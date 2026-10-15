> [!IMPORTANT] ACID isn't a checklist you tick off — it's the answer to a very specific engineering problem: **how do multiple database operations behave as one trustworthy unit, even when things crash, run concurrently, or fail halfway through?** Understanding ACID means understanding _what breaks without it_, not memorizing four letters.

---

## Core Learning Objective

After this note, you should be able to:

- Explain ACID in 30 seconds, 2 minutes, and 5 minutes
- Define Atomicity, Consistency, Isolation, and Durability precisely, and explain _why_ each exists
- Walk through a bank-transfer example showing exactly what fails without each property
- Explain dirty reads, non-repeatable reads, phantom reads, and lost updates with timelines
- Compare isolation levels and know which anomalies each permits
- Explain MVCC and how it relates to isolation
- Distinguish ACID Consistency from CAP Consistency without hesitation
- Explain, at a high level, how databases actually implement ACID (WAL, locking, MVCC, recovery)
- Reason about ACID trade-offs in system-design interviews across multiple realistic scenarios

---

## Why Transactions Exist — Start With the Problem

A **database transaction** is a sequence of one or more operations that must be treated as a single logical unit of work — either all of it happens, or none of it does.

Why does this matter? Consider transferring money:

```text
Transfer £100 from Alice → Bob

1. Debit £100 from Alice
2. Credit £100 to Bob
```

These are **two separate database writes**. Now imagine the system crashes right after step 1 completes but before step 2 runs:

```text
Alice: £1000 → £900
Bob:   £500  → £500   ❌  (never received the £100)
```

£100 has simply vanished from the system. This is not a hypothetical edge case — crashes, network failures, and concurrent access are a _guaranteed_ part of running any real system at scale. Without some guarantee wrapping these two writes together, this failure mode is inevitable, not just possible.

This is exactly the problem a **transaction** solves: the database guarantees that steps 1 and 2 either **both** happen or **neither** happens — no matter what fails in between.

But atomicity alone isn't enough. What if two customers are transferring money to/from the same account at the same instant? What if the machine loses power _after_ the transfer is confirmed to the user? ACID is the full set of four guarantees needed to make transactions actually trustworthy under crashes, concurrency, and hardware failure.

---

## ACID Overview

|Property|Meaning|Problem It Prevents|Key Mechanism/Concept|
|---|---|---|---|
|**Atomicity**|A transaction's operations happen entirely, or not at all|Partial updates leaving data in a broken intermediate state|Transaction log, undo records, rollback|
|**Consistency**|A transaction moves the database from one valid state to another, respecting all defined constraints|Data violating business rules/integrity constraints|Constraint checking (PK, FK, CHECK, NOT NULL)|
|**Isolation**|Concurrent transactions don't improperly interfere with each other|Dirty reads, lost updates, and other concurrency anomalies|Locking, MVCC, isolation levels|
|**Durability**|Once a transaction commits, its effects survive subsequent failures|Losing confirmed data after a crash|Write-Ahead Logging (WAL), fsync, checkpoints, redo|

> [!TIP] One-Line Mental Model
> 
> ```text
> Atomicity   → All or nothing
> Consistency → Valid state → valid state
> Isolation   → Concurrent transactions don't improperly interfere
> Durability  → Committed data survives failures
> ```

---

## Atomicity

**Atomicity** means a transaction's set of operations is treated as an indivisible unit — it either **commits** entirely, or is **rolled back** entirely, leaving no partial trace.

### Transaction Boundaries

```sql
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE name = 'Alice';
UPDATE accounts SET balance = balance + 100 WHERE name = 'Bob';

COMMIT;
```

- **`BEGIN TRANSACTION`** marks the start of the atomic unit.
- **`COMMIT`** tells the database: "make all these changes permanent, together."
- **`ROLLBACK`** tells the database: "undo everything since `BEGIN`, as if none of it happened."

### What Happens on Failure

```text
BEGIN
   ↓
Debit Alice
   ↓
Database crash
   ↓
ROLLBACK / recovery
   ↓
Transaction does not partially persist
```

If the crash happens after "Debit Alice" but before "Credit Bob," the database's **recovery process** (run automatically on restart) detects the incomplete transaction and **undoes** the debit — restoring Alice's original balance. The transaction never partially exists from an outside observer's point of view.

### How Atomicity Is Achieved (High Level)

- **Transaction/undo logs** — before modifying data, the database records enough information to reverse the change if needed.
- **Recovery mechanisms** — on restart after a crash, the database scans its logs, identifies incomplete transactions, and rolls them back automatically.

> [!WARNING] Not every database implements atomicity identically. The _mechanism_ (undo logs, shadow paging, log-structured approaches) varies by engine — but the _guarantee_ (all-or-nothing) is what ACID actually promises. Don't assume implementation details transfer across engines.

---

## Consistency

> [!IMPORTANT] Precise Definition **ACID Consistency**: A successful transaction moves the database from one **valid state** to another **valid state**, preserving all defined integrity constraints. It says nothing about timing, freshness across nodes, or what other transactions see — only that constraints are never violated.

### Constraints That Define "Valid"

- **Primary keys** — no duplicate identifiers
- **Foreign keys** — references must point to existing rows
- **Unique constraints** — no duplicate values in a designated column
- **NOT NULL constraints** — required fields must have a value
- **CHECK constraints** — arbitrary business rules, e.g.:

```sql
ALTER TABLE accounts ADD CONSTRAINT chk_balance CHECK (balance >= 0);
```

- **Application-level invariants** — rules the application itself enforces, which the database can't always encode directly (e.g., "total debits must equal total credits across a batch").

If a transaction would violate any of these, the database **rejects the write and rolls back** rather than allowing the database to enter an invalid state.

### ⚠️ Critical Interview Distinction

> [!WARNING] ACID Consistency ≠ CAP Consistency This is one of the most commonly confused pairs of terms in database interviews.

|ACID Consistency|CAP Consistency|
|---|---|
|About a **single transaction** respecting declared constraints|About **every replica**, read at any time, reflecting the most recent write|
|Concerned with **data validity** (does it violate a rule?)|Concerned with **data freshness** across nodes (is it the latest value?)|
|Applies even to a **single-node** database with no replication|Only meaningful in a **distributed, multi-node** system|
|Enforced via constraint checking at write time|Enforced (or relaxed) via replication/coordination strategy|
|A transaction can be ACID-consistent and still return **stale data** from a lagging replica|A system can be CAP-consistent and still **violate** an application-level business rule if that rule was never encoded as a constraint|

> [!TIP] Interview Angle If an interviewer asks "is this system consistent?", the correct first move is to ask **"consistent in which sense — ACID or CAP?"** — this single clarifying question signals real depth. See [[CAP Theorem]] for the full treatment of CAP consistency.

---

## Isolation

> [!IMPORTANT] This is the deepest and most interview-tested section of this note. Take your time here.

### The Problem

What happens when multiple transactions execute **concurrently** against the same data?

```text
Transaction T1
Transaction T2
       ↓
Both access the same data
```

If transactions always ran **serially** — one fully completing before the next begins — there would be no isolation problem at all. But serial execution wastes massive throughput: real databases handle thousands of concurrent transactions per second, and forcing them to queue one-at-a-time would be unacceptably slow.

So databases run transactions **concurrently**, and **Isolation** is the guarantee that this concurrency doesn't produce _incorrect_ results — specifically, that concurrent execution behaves _as if_ it were some valid serial order (this is formalized later under [[#Serializability]]).

---

## Concurrency Anomalies

These are the specific ways things can go wrong without adequate isolation.

### Dirty Read

**Problem:** A transaction reads data written by another transaction that has **not yet committed** — and might never commit.

**Timeline:**

```text
T1: UPDATE accounts SET balance = 900 WHERE id = 1;   -- not yet committed
T2: SELECT balance FROM accounts WHERE id = 1;         -- reads 900
T1: ROLLBACK;                                          -- balance reverts to original
```

**Why it happens:** T2 was allowed to see T1's uncommitted change.

**How isolation prevents it:** Any isolation level stronger than Read Uncommitted ensures reads only see **committed** data.

### Non-Repeatable Read

**Problem:** A transaction reads the same row **twice**, and gets two different **committed** values, because another transaction modified and committed a change in between.

**Timeline:**

```text
T1: SELECT balance FROM accounts WHERE id = 1;   -- returns 1000
T2: UPDATE accounts SET balance = 900 WHERE id = 1;
T2: COMMIT;
T1: SELECT balance FROM accounts WHERE id = 1;   -- returns 900 (different!)
```

**Why it happens:** T1's isolation level didn't guarantee a stable snapshot across its own multiple reads.

**How isolation prevents it:** Repeatable Read (and stronger) guarantees that once T1 reads a row, subsequent reads of that same row within T1 return the same value.

### Phantom Read

**Problem:** A transaction re-runs the **same range query** and sees a **different set of rows**, because another transaction inserted or deleted rows matching the query's condition.

**Timeline:**

```text
T1: SELECT * FROM accounts WHERE balance > 500;   -- returns 3 rows
T2: INSERT INTO accounts (balance) VALUES (600);
T2: COMMIT;
T1: SELECT * FROM accounts WHERE balance > 500;   -- returns 4 rows (phantom!)
```

**Why it happens:** Repeatable Read guarantees existing rows won't _change_ underneath you, but by default doesn't necessarily prevent _new matching rows_ from appearing — that requires additional mechanisms (like gap locking).

**How isolation prevents it:** Serializable isolation (or engine-specific mechanisms like next-key/gap locks) prevents new rows from being inserted into a range another transaction has already queried.

### Lost Update

**Problem:** Two concurrent transactions both read the same value, both compute a new value based on it, and the second write **silently overwrites** the first — losing an update.

**Timeline:**

```text
T1: SELECT balance FROM accounts WHERE id = 1;   -- reads 1000
T2: SELECT balance FROM accounts WHERE id = 1;   -- reads 1000
T1: UPDATE accounts SET balance = 1000 + 100;    -- writes 1100
T2: UPDATE accounts SET balance = 1000 + 50;     -- writes 1050 (overwrites T1's update!)
```

**Why it happens:** Neither transaction was aware of the other's concurrent write — the final value reflects only T2's calculation, silently discarding T1's £100 addition.

**How isolation prevents it:** Proper locking (`SELECT ... FOR UPDATE`) or optimistic concurrency control (version checks) ensures the second write detects the conflict rather than blindly overwriting.

> [!WARNING] These four anomalies are **distinct failure modes**, not interchangeable synonyms for "concurrency bug." Interviewers frequently test whether you can name which specific anomaly a given scenario represents.

---

## Isolation Levels

|Isolation Level|Dirty Read|Non-Repeatable Read|Phantom Read|Concurrency|Typical Use|
|---|--:|--:|--:|---|---|
|**Read Uncommitted**|Possible|Possible|Possible|Highest|Rare in practice; some analytical/reporting queries tolerant of dirty data|
|**Read Committed**|Prevented|Possible|Possible|High|Common default (PostgreSQL, Oracle, SQL Server default) — good balance for most OLTP workloads|
|**Repeatable Read**|Prevented|Prevented|Possible (varies by engine)|Medium|MySQL/InnoDB's default; suitable when a transaction must see a stable view of rows it has read|
|**Serializable**|Prevented|Prevented|Prevented|Lowest|Financial reconciliation, anything requiring true correctness guarantees regardless of cost|

> [!WARNING] Engine Behavior Varies The SQL standard defines these anomaly guarantees abstractly, but **real engines diverge**. Most notably: MySQL's InnoDB **Repeatable Read** actually prevents most phantom reads too (via gap/next-key locking), which is _stronger_ than the SQL standard strictly requires at that level. PostgreSQL's Repeatable Read is implemented via snapshot isolation, with different (though also strong) anomaly-prevention characteristics. **Always name the specific database engine** when discussing isolation-level guarantees in an interview — don't assume the SQL standard table applies universally.

---

## MVCC (Multi-Version Concurrency Control)

**Why it exists:** Traditional locking forces readers and writers to block each other. MVCC lets databases avoid this by keeping **multiple versions** of each row.

- When a transaction writes, it doesn't overwrite the old value in place — it creates a **new version**, while the old version remains available.
- Each transaction reads from a **consistent snapshot** — the version of the data that was current at (roughly) the moment its snapshot was taken — rather than the "live," possibly-mid-update state.
- This means **readers don't block writers, and writers don't block readers** in most cases, dramatically improving concurrency versus pure lock-based approaches.

> [!IMPORTANT] **MVCC does not automatically mean "fully serializable."** MVCC solves _read consistency_ elegantly, but write-write conflicts (like the Lost Update anomaly above) still require additional mechanisms — such as detecting conflicting writes at commit time and aborting one transaction, or explicit locking (`SELECT ... FOR UPDATE`). Snapshot isolation (what MVCC typically provides by default) is a real, useful isolation guarantee, but it is **not identical to Serializable** — certain anomalies (like write skew) can still occur under plain snapshot isolation.

---

## Serializability

**Serial execution** means transactions run one completely after another, with zero overlap — trivially correct, but slow.

**Serializability** is a guarantee that concurrent execution produces a result **equivalent to some valid serial ordering** of those transactions — even though they may have actually run interleaved, in parallel, for performance.

```text
Serial execution              vs        Serializable concurrent execution
T1 fully runs, then T2 runs             T1 and T2 run interleaved, but the
                                          final result is identical to some
                                          valid serial order (T1 then T2, or
                                          T2 then T1)
```

> [!TIP] Serializability is about the **outcome**, not the literal execution order. The database is free to interleave operations for performance, as long as it can guarantee the result is indistinguishable from _some_ serial order. This distinction — outcome-equivalence, not literal sequencing — is exactly what interviewers are checking for when they ask "does Serializable mean transactions run one at a time?" (Answer: no.)

---

## Durability

**Durability** guarantees that once a transaction **commits**, its effects survive subsequent failures — the database will never "forget" a confirmed transaction.

```text
COMMIT
  ↓
Power failure
  ↓
Database restarts
  ↓
Committed transaction must remain
```

### High-Level Mechanisms

- **Write-Ahead Logging (WAL)** — before a change is applied to the actual data files, it's first written to a durable, append-only log. If the system crashes, the log is **replayed (redo)** on restart to reconstruct any committed-but-not-yet-fully-applied changes.
- **fsync / persistent storage** — the log itself must actually be flushed to durable storage (not just an OS buffer) before the commit is acknowledged to the client, or the "durability" guarantee is hollow.
- **Checkpoints** — periodic snapshots that let the database recover faster by not needing to replay the entire log history from the beginning of time.
- **Crash recovery / Redo** — on restart, the database uses the WAL to redo any committed transactions that hadn't yet been fully persisted to the main data files.

> [!WARNING] Durability Scope Matters "Committed" does **not** automatically mean the data has been written to every replica, or to a different geographic region. Be precise about **which** durability guarantee is in play:
> 
> |Type|What It Guarantees|
> |---|---|
> |**Database (local) durability**|The commit survives a crash/restart of _this_ node, because the WAL was flushed to local persistent storage|
> |**Replication durability**|The commit survives the _loss of the primary node entirely_, because at least one replica has also confirmed the write (requires synchronous or semi-synchronous replication)|
> |**Cross-region durability**|The commit survives the loss of an _entire data center/region_, requiring geographically distributed replication|
> 
> A database can be fully "durable" in the local sense while still losing data if the entire machine (not just the process) is destroyed before replication catches up. This nuance connects directly to [[CAP Theorem]] and [[Replication]].

---

## What Happens When Things Go Wrong?

|Failure Scenario|Relevant ACID Properties|What Happens|
|---|---|---|
|**Application crash**|Atomicity|If `COMMIT` was never sent, the transaction remains open/uncommitted; the database eventually times it out and rolls it back|
|**Database process crash**|Atomicity, Durability|On restart, the recovery process uses the WAL to redo committed transactions and undo incomplete ones|
|**Machine crash**|Atomicity, Durability, (Replication)|Local WAL-based recovery works if storage survives; if the machine/disk is destroyed, only replicated data survives|
|**Power failure**|Durability|If fsync had confirmed the WAL write before power loss, the commit survives; if not (e.g., unsafe caching), it may be lost — this is why proper fsync configuration matters|
|**Transaction timeout**|Atomicity|The transaction is rolled back as if it never started|
|**Deadlock**|Isolation, Atomicity|The database detects the cycle and forcibly rolls back one transaction (the "victim") to break the deadlock|
|**Network failure (client ↔ database)**|Atomicity, application-layer concerns|The transaction's fate (committed or not) may be ambiguous to the client — this is why idempotent retries matter at the application layer|
|**Replica failure**|Durability (replication scope), Availability|Depends entirely on replication mode — synchronous replication may block the primary; asynchronous replication risks losing the most recent commits if the primary also fails|

---

## ACID Implementation — High Level

```text
Transaction Manager
        ↓
Concurrency Control
        ↓
Locking / MVCC
        ↓
Write-Ahead Log
        ↓
Recovery
        ↓
Persistent Storage
```

- **Transaction Manager** — coordinates `BEGIN`/`COMMIT`/`ROLLBACK`, tracking each transaction's state.
- **Concurrency Control** — decides how concurrent transactions interact: via **locking** (Two-Phase Locking, or 2PL — acquire all locks before releasing any, ensuring serializability) or via **MVCC** (snapshot-based, minimizing blocking).
- **Write-Ahead Log (WAL)** — the durability backbone: every change is logged before being applied.
- **Undo/Redo** — undo information supports rollback (Atomicity); redo information supports crash recovery (Durability).
- **Checkpoints** — periodic sync points that bound how much log history must be replayed after a crash.

> [!TIP] Interview Framing If asked "how does a database actually provide ACID," the strongest answer names the **specific mechanism per property**: atomicity via undo logs/rollback, consistency via constraint enforcement, isolation via locking/MVCC, durability via WAL + fsync + redo recovery. Naming mechanisms, not just properties, is what separates a strong answer from a memorized one.

---

## Deadlocks

**Deadlock:** two (or more) transactions each hold a lock the other needs, forming a cycle with no way forward.

```text
T1 locks A → waits for B
T2 locks B → waits for A
```

- **Why locks cause deadlocks:** if transactions acquire locks in different orders, a circular wait can form.
- **Detection:** most databases maintain a "wait-for" graph and periodically check for cycles; when found, a cycle is broken.
- **Prevention/avoidance (high level):** enforcing a consistent lock-acquisition order across all transactions avoids circular waits entirely; timeouts are a simpler (if blunter) alternative.
- **Why the database aborts one transaction:** breaking the cycle requires sacrificing one transaction's progress — the database typically picks the "cheaper" one to roll back (e.g., the one that has done less work).
- **Why retry logic matters:** because the aborted transaction's changes are rolled back, the **application** must be prepared to detect this failure and retry the transaction — the database does not automatically retry on your behalf.

---

## ACID vs BASE

|Dimension|ACID|BASE|
|---|---|---|
|Primary goal|Strong correctness guarantees, even under concurrency/failure|High availability and scalability, tolerating temporary inconsistency|
|Consistency approach|Strict — constraints always hold, reads always reflect committed state|Eventual — replicas converge over time, but may briefly diverge|
|Availability|May sacrifice availability to preserve correctness (e.g., blocking during coordination)|Prioritizes staying responsive, even with stale data|
|Transaction model|Well-defined transaction boundaries with strong guarantees|Often lacks traditional multi-operation transactions; relies on idempotency and reconciliation instead|
|Typical systems|Traditional RDBMS (PostgreSQL, MySQL/InnoDB, Oracle, SQL Server)|Many NoSQL/distributed systems (Dynamo-style stores, some Cassandra configurations)|
|Trade-offs|Stronger correctness, at the cost of coordination overhead/latency, and sometimes availability during partitions|Higher availability/throughput, at the cost of applications needing to handle temporary inconsistency|

> [!WARNING] Neither model is universally superior. **ACID isn't "correct" and BASE isn't "sloppy"** — they represent different, deliberate points on the correctness-vs-availability spectrum, chosen based on what the workload actually needs. A social media "like" counter doesn't need ACID; a bank ledger doesn't tolerate BASE. See [[BASE]] for the full treatment.

---

## ACID vs CAP

> [!IMPORTANT] ACID and CAP are **not competing definitions** — they address different problems and can both apply to the same distributed database simultaneously.

|Aspect|ACID|CAP|
|---|---|---|
|What problem does it address?|Correctness of a **single transaction's** operations, even under concurrency and failure|Trade-off a **distributed system** must make when nodes can't communicate (a network partition)|
|Scope|Primarily transaction-level; can be single-node or (with more effort) distributed|Inherently about multi-node, distributed systems|
|Key guarantees|Atomicity, Consistency (constraints), Isolation, Durability|Consistency (freshness across replicas) vs. Availability, during a partition|
|Overlap|A distributed database can be internally ACID for its local transactions, while still facing CAP trade-offs when those transactions must span partitioned nodes|CAP's "C" is a narrower freshness guarantee than ACID's "C," but both use the word "consistency" — a frequent source of confusion (see the earlier warning callout)|
|Where they differ|ACID is about **transaction correctness**|CAP is about **availability behavior during network failure**|

**Why a distributed database can involve both:** Consider a globally distributed database processing a transaction that touches data on nodes in two different regions. The database still wants ACID guarantees for that transaction (atomicity across both regions' writes, isolation from concurrent transactions). But _achieving_ that ACID guarantee across a network requires coordination (e.g., Two-Phase Commit or consensus) — and if a partition occurs mid-coordination, the system faces exactly the CAP trade-off: block/abort to preserve correctness (favoring the "C" side of both ACID and CAP), or proceed with reduced guarantees to stay available.

> [!TIP] System-Design Interview Framing _"ACID tells you what guarantees a transaction should provide. CAP tells you what happens to those guarantees when the network fails. In a distributed system, you often need to reason about both: what correctness does this operation need (ACID), and what happens to that correctness requirement during a partition (CAP)?"_

---

## ACID in Distributed Databases

Single-node ACID is relatively well-understood and efficiently implementable. Once data spans multiple nodes, new complexity appears:

```text
Single-node transactions
        ↓
Replication
        ↓
Sharding
        ↓
Distributed transactions
        ↓
Consensus / coordination
        ↓
Distributed system trade-offs
```

- **Distributed transactions** — a transaction spanning multiple nodes/shards must coordinate to guarantee atomicity across all of them.
- **Two-Phase Commit (2PC)** — a classic coordination protocol: a coordinator asks all participants to "prepare" (ensure they _can_ commit), and only if all agree does it tell everyone to actually commit. This provides atomicity across nodes, but the coordinator becomes a bottleneck/single point of failure, and participants can block indefinitely if the coordinator crashes mid-protocol.
- **Consensus** (Paxos, Raft) — used by modern distributed databases to agree on a single, consistent ordering of operations across replicas, forming the foundation for stronger distributed guarantees than 2PC alone typically provides.
- **Replication & Quorum** — how many nodes must acknowledge a write before it's considered durable/committed directly affects both durability guarantees and availability during a partition.
- **Cross-region transactions** — the highest-latency, highest-coordination-cost case; some systems (e.g., Google Spanner) use synchronized clocks and consensus to provide strong guarantees globally, at a real latency cost.

> [!TIP] This is intentionally a **bridge**, not a full treatment — see [[CAP Theorem]], [[Distributed Systems]], [[Replication]], [[Database Sharding]], [[Consensus]], and [[Quorum]] for deeper dives into each of these mechanisms.

---

## Real-World Database Examples

> [!WARNING] Avoid blanket claims like "Database X is ACID." Transactional guarantees depend on **storage engine, transaction scope, and configuration** — always name these specifics.

### PostgreSQL

Provides full ACID transactions by default, using MVCC for isolation (implementing snapshot isolation under Repeatable Read, and a true serializable mode via Serializable Snapshot Isolation). Its Read Committed default is well-suited to most OLTP workloads.

### MySQL / InnoDB

The **InnoDB storage engine** provides full ACID guarantees (the older MyISAM engine does **not** — no transactions at all), with Repeatable Read as the default isolation level, additionally mitigating most phantom reads via gap/next-key locking — a stronger guarantee than the SQL standard strictly requires at that level.

### SQL Server

Provides full ACID transactions, with configurable isolation levels including a **Snapshot Isolation** mode (MVCC-based) as an alternative to traditional locking-based isolation.

### Oracle

Provides full ACID transactions with a long history of sophisticated MVCC-based concurrency control (Oracle popularized much of the "readers never block writers" MVCC approach still influential today).

### MongoDB

Modern MongoDB (4.0+) supports **multi-document ACID transactions**, including across sharded clusters (4.2+). Earlier versions only guaranteed atomicity at the **single-document** level. The key nuance: multi-document transactional support is real but was added later in MongoDB's history, and using it effectively still requires understanding its specific consistency/isolation configuration — don't assume it behaves identically to a traditional RDBMS's default transaction model.

### Distributed SQL Databases (e.g., Google Spanner, CockroachDB)

Aim to provide full ACID guarantees **even across globally distributed nodes**, using consensus protocols and (in Spanner's case) synchronized clocks (TrueTime) to achieve external consistency. This comes at a real latency cost for cross-region transactions — a direct, practical instance of the ACID/CAP interaction discussed above.

---

## ACID in System Design Interviews

### 1. Payment System

- **Money movement:** debit and credit must be atomic — the exact bank-transfer example from earlier.
- **Idempotency:** because network failures can make a client unsure whether a request succeeded, payment APIs must be designed so that retrying an already-processed request doesn't double-charge — this is a separate concern from ACID, but works _with_ it (an idempotency key check inside the same atomic transaction).
- **Transaction boundaries:** the debit and credit should be in a single transaction; anything that can be handled asynchronously (e.g., sending a confirmation email) should be _outside_ that transaction boundary to keep it small and fast.
- **Failure handling:** on crash, the transaction rolls back cleanly (Atomicity); the client must handle ambiguous responses via idempotent retries.

### 2. E-commerce Order Placement

- **Inventory:** decrementing stock and creating an order should ideally be atomic to avoid overselling.
- **Payment:** often a _separate_ system/transaction (frequently a third-party payment processor) — meaning true cross-system atomicity isn't achievable, so a **saga pattern** (a sequence of local transactions with compensating actions on failure) is commonly used instead of a single distributed transaction.
- **Consistency requirements:** inventory counts need strong consistency to avoid overselling; "recently viewed items" or recommendation data can tolerate eventual consistency.
- **Distributed transaction challenges:** spanning the order database, inventory database, and payment gateway in one ACID transaction is often impractical — real systems compose multiple smaller transactions with careful failure/compensation handling instead.

### 3. Banking System

- **Account balances:** must never go negative (a CHECK constraint) and must never be lost mid-transfer (Atomicity).
- **Transfers:** the canonical ACID example — debit and credit as one unit.
- **Concurrency:** many users may access/modify the same account concurrently — Isolation (often via row-level locking or careful MVCC configuration) prevents lost updates.
- **Correctness:** typically favors stronger isolation levels (sometimes Serializable) for balance-critical operations, accepting the throughput cost as justified by the correctness requirement.

### 4. Social Media / Content System

- **Strict ACID often unnecessary** for things like: like counts, view counts, follower counts, feed ranking — brief staleness or approximate counts are acceptable trade-offs for higher throughput/availability.
- Strict guarantees _may_ still be needed for narrower slices: e.g., a user's account credentials, payment/subscription status, or content moderation flags — showing again that **ACID requirements should be decided per data type, not per entire system**.

For each scenario, the reasoning always follows:

```text
What must be strongly correct?
What can be eventually consistent?
Where is a transaction boundary?
What happens during failure?
What happens under concurrency?
What trade-offs exist?
```

---

## How to Think About ACID in an Interview

> [!INTERVIEW] Don't just say "we need ACID." That's a conclusion without reasoning — interviewers want to see the reasoning itself.

Work through:

```text
1. What data is changing?
2. What operations must succeed together?
3. What invariants must always hold?
4. What happens if the process crashes?
5. What happens if two users update simultaneously?
6. What isolation level is actually required?
7. What happens after COMMIT?
8. Is the transaction local or distributed?
9. What latency/availability trade-offs are acceptable?
10. Can the business tolerate eventual consistency?
```

Answering these questions **in order**, out loud, for the specific data in question, is what a strong interview answer actually sounds like — far more convincing than declaring "I'd use a transaction here" without justification.

---

## Common Interview Traps

```text
❌ "ACID means the database never loses data."
✅ ACID guarantees apply to what's covered by its durability scope (local commit, or replicated, depending on config) — it doesn't protect against, e.g., an entire data center being destroyed unless replication/geo-durability was also configured.
🎯 Interviewers use this to check whether you understand durability has a defined scope, not an absolute one.
```

```text
❌ "Consistency means every read gets the latest data."
✅ That's CAP consistency (freshness). ACID consistency means the database never violates its declared constraints — a completely different guarantee.
🎯 This exact confusion is one of the most common interview red flags.
```

```text
❌ "ACID consistency and CAP consistency are the same."
✅ They are different axes entirely — see the dedicated comparison table above.
🎯 Interviewers specifically probe this to filter shallow, buzzword-level understanding.
```

```text
❌ "Serializable means transactions execute one at a time."
✅ Serializable means the result is equivalent to some serial order — transactions can still run concurrently under the hood for performance.
🎯 Tests whether you understand outcome-equivalence vs. literal execution order.
```

```text
❌ "Read Committed prevents every concurrency anomaly."
✅ Read Committed only prevents dirty reads — non-repeatable reads and phantom reads are still possible at that level.
🎯 Tests whether you actually know the isolation-level anomaly table, not just the names of the levels.
```

```text
❌ "Durability means data exists on every replica."
✅ Durability by default typically means the local commit survives a local crash/restart; surviving the loss of an entire node requires replication durability, a distinct, stronger guarantee.
🎯 Tests whether you understand durability has layered scope (local/replication/cross-region).
```

```text
❌ "ACID automatically solves distributed transactions."
✅ ACID defines the guarantees; achieving them across multiple nodes requires additional coordination mechanisms (2PC, consensus), each with real trade-offs and failure modes.
🎯 Tests whether you can bridge single-node theory to distributed-systems practice.
```

```text
❌ "MongoDB is not ACID."
✅ Modern MongoDB supports multi-document ACID transactions (single-document atomicity has always been guaranteed); the accurate statement is version- and configuration-dependent, not a blanket "no."
🎯 Tests whether your knowledge is current and whether you avoid outdated blanket claims.
```

```text
❌ "NoSQL databases cannot provide ACID transactions."
✅ Many modern NoSQL systems provide various forms of transactional guarantees (MongoDB's multi-document transactions, for example); "NoSQL" describes a data model choice, not an inherent lack of transactional support.
🎯 Tests whether you're working from outdated categorical assumptions rather than current system capabilities.
```

```text
❌ "More isolation is always better."
✅ Higher isolation levels (especially Serializable) reduce concurrency and throughput — the "right" level is the weakest one that still prevents the anomalies your specific workload can't tolerate.
🎯 Tests trade-off reasoning, not a reflexive "stronger is always safer" instinct.
```

---

## Mental Models

### Atomicity

> All or nothing.

### Consistency

> Valid state → valid state.

### Isolation

> Concurrent transactions should behave according to the chosen isolation guarantees — not "no interference at all," but "controlled, specified interference."

### Durability

> Once committed, the transaction survives the failures covered by the database's durability guarantees — a defined scope, not an absolute promise.

> [!TIP] One-Line ACID Mental Model **"ACID is what lets you treat a sequence of database writes as a single trustworthy fact, even when the world crashes, runs concurrently, or fails halfway through."**

> [!TIP] Bank-Transfer Mental Model Every ACID property maps to a specific way a bank transfer could go wrong without it: Atomicity → money vanishing mid-transfer; Consistency → an account going negative; Isolation → two concurrent transfers silently overwriting each other; Durability → a confirmed transfer disappearing after a crash.

> [!TIP] Interview Mental Model Every ACID interview question is secretly asking: **"Can you name the specific failure this property prevents, and the specific mechanism that prevents it?"**

> [!TIP] Failure-Recovery Mental Model On any crash, the database asks two questions using its logs: _"Which transactions were committed but not yet fully applied? Redo them. Which transactions were incomplete? Undo them."_ Everything else in ACID's implementation supports answering these two questions correctly.

---

## Quick Revision

### ACID Definition

The four guarantees — Atomicity, Consistency, Isolation, Durability — that make database transactions behave as trustworthy, indivisible units even under concurrency and failure.

### Atomicity

All-or-nothing; achieved via undo logs and rollback/recovery.

### Consistency

Valid state → valid state, via constraint enforcement (PK, FK, UNIQUE, CHECK, NOT NULL). **Not the same as CAP consistency.**

### Isolation

Concurrent transactions don't improperly interfere; achieved via locking and/or MVCC; strength is configurable via isolation level.

### Durability

Committed data survives failures within a defined scope (local/replication/cross-region); achieved via WAL + fsync + redo recovery.

### Transaction Definition

A sequence of operations executed as a single logical, atomic unit of work, bounded by `BEGIN`/`COMMIT`/`ROLLBACK`.

### Four Major Concurrency Anomalies

- **Dirty Read** — reading uncommitted data
- **Non-Repeatable Read** — same row, different committed values across two reads
- **Phantom Read** — same range query, different row set across two reads
- **Lost Update** — one transaction's write silently overwritten by another's

### Isolation Levels

Read Uncommitted → Read Committed → Repeatable Read → Serializable, each preventing progressively more anomalies at the cost of concurrency. Engine behavior varies — always name the specific database.

### MVCC

Multiple row versions let readers use consistent snapshots without blocking writers; solves read consistency elegantly, but write-write conflicts still need separate handling; snapshot isolation ≠ automatically Serializable.

### WAL

Write-Ahead Logging: log the change durably before applying/acknowledging it, enabling crash recovery via redo.

### Serializability

Concurrent execution's result must be equivalent to _some_ serial order — not literal one-at-a-time execution.

### ACID vs CAP

Different problems: ACID = transaction correctness; CAP = availability/consistency trade-off during a network partition. Both can apply to the same distributed database, at different layers.

### ACID vs BASE

ACID = strong correctness, potentially at the cost of availability/latency; BASE = high availability, tolerating eventual consistency. Neither is universally "better" — it's workload-dependent.

### 5 Interview Takeaways

1. Always distinguish **ACID consistency** (constraint validity) from **CAP consistency** (cross-node freshness).
2. Name the **specific mechanism** behind each property (undo logs, WAL, MVCC, locking) — not just the definition.
3. **Serializable ≠ literally sequential** — it's about outcome-equivalence to some serial order.
4. Durability has **layered scope** — local, replicated, cross-region — never assume the strongest by default.
5. ACID requirements should be reasoned about **per data type/operation**, not applied uniformly across an entire system.

---

## 🎯 Interview Cheat Sheet

```text
ACID =
A → All or nothing
C → Valid state → valid state
I → Controlled concurrent execution
D → Committed changes survive covered failures
```

### Must Know

- The four ACID properties and the specific mechanism behind each
- The four concurrency anomalies (dirty read, non-repeatable read, phantom read, lost update)
- The four isolation levels and which anomalies each permits
- ACID Consistency ≠ CAP Consistency
- MVCC's role in isolation, and its limits (not automatically Serializable)
- WAL's role in durability
- What Two-Phase Commit and the Saga pattern are for

### Must Be Able to Explain

- The bank-transfer example end-to-end, mapping each failure mode to an ACID property
- Why Serializable doesn't mean literal one-at-a-time execution
- Why durability's "commit survives failure" claim has a defined scope
- How a deadlock forms and how a database resolves it
- The difference between ACID and BASE as trade-offs, not good/bad labels
- The difference between ACID and CAP as different problems, not synonyms

### Must Be Able to Apply

- Payment system: atomic debit/credit, idempotency, transaction boundaries
- E-commerce checkout: why full ACID across services is often impractical, and how Sagas help
- Banking system: isolation-level choice for balance-critical operations
- Social platform: identifying which data needs ACID vs. which can tolerate eventual consistency

### Common Trap

> The single most damaging mistake: **conflating ACID Consistency with CAP Consistency.** If you remember nothing else from this note, remember this distinction — it's the fastest way interviewers separate memorized answers from real understanding.

---

## Related Notes

- [[What is DBMS]]
- [[RDBMS]]
- [[CAP Theorem]]
- [[BASE]]
- [[OLTP vs OLAP]]
- [[Distributed Systems]]
- [[Concurrency Control]]
- [[Replication]]
- [[Database Sharding]]
- [[Consensus]]
- [[Quorum]]
- [[System Design]]