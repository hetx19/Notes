A **string** in the [[STL (Standard Template Library)]] is a container provided by the `<string>` library that stores and manipulates a sequence of characters.

- Dynamic size (can grow or shrink)
- Stores characters contiguously in memory    
- Supports random access using indexing
- Provides many built-in functions for string manipulation
- Implemented as `basic_string<char>`

- Time Complexity:    
    - Access by index: **O(1)**
    - Append at end: **O(1)** amortized
    - Insert/Erase in middle: **O(n)**

### Syntax

```cpp
string str;
```

#### Example

```cpp
string s = "Hello";
string name("Het");
```

---

#### Functions in string:

- length() / size()  
    Returns the number of characters in the string.
    
    Syntax

```cpp
str.length();
str.size();
```

Example

```cpp
string s = "Hello";

cout << s.length(); // 5
cout << s.size();   // 5
```

- empty()  
    Checks whether the string is empty.
    
    Syntax

```cpp
str.empty();
```

Example

```cpp
string s;

if (s.empty()) {
    cout << "String is empty";
}
```

- clear()  
    Removes all characters from the string.
    
    Syntax
    
```cpp
str.clear();
```

Example

```cpp
string s = "Hello";
s.clear();

cout << s.size(); // 0
```

- push_back()  
    Appends a character at the end of the string.
    
    Syntax

```cpp
str.push_back(ch);
```

Example

```cpp
string s = "Hell";
s.push_back('o');

cout << s; // Hello
```

- pop_back()  
    Removes the last character from the string.
    
    Syntax

```cpp
str.pop_back();
```

Example

```cpp
string s = "Hello";
s.pop_back();

cout << s; // Hell
```

- front()  
    Returns the first character of the string.
    
    Syntax

```cpp
str.front();
```

Example

```cpp
string s = "Hello";
cout << s.front(); // H
```

- back()  
    Returns the last character of the string.
    
    Syntax

```cpp
str.back();
```

Example

```cpp
string s = "Hello";
cout << s.back(); // o
```

- at()  
    Returns the character at a specific index with bounds checking.
    
    Syntax

```cpp
str.at(index);
```

Example

```cpp
string s = "Hello";
cout << s.at(1); // e
```

- substr()  
    Returns a substring starting from a given position.
    
    Syntax

```cpp
str.substr(start, length);
```

Example

```cpp
string s = "Programming";
cout << s.substr(3, 4); // gram
```

- find()  
    Returns the index of the first occurrence of a substring.  
    If not found, returns `string::npos`.
    
    Syntax

```cpp
str.find(substring);
```

Example

```cpp
string s = "Hello World";
cout << s.find("World"); // 6
```

```cpp
if(s.find("Java") == string::npos) {
    cout << "Not Found";
}
```

- rfind()  
    Returns the index of the last occurrence of a substring.
    
    Syntax

```cpp
str.rfind(substring);
```

Example

```cpp
string s = "abcabc";
cout << s.rfind("abc"); // 3
```

- erase()  
    Removes characters from the string.
    
    Syntax

```cpp
str.erase(pos, count);
```

Example

```cpp
string s = "Hello World";
s.erase(5, 1);

cout << s; // HelloWorld
```

- insert()  
    Inserts characters at a specified position.
    
    Syntax

```cpp
str.insert(pos, text);
```

Example

```cpp
string s = "Helo";
s.insert(3, "l");

cout << s; // Hello
```

- replace()  
    Replaces part of the string with another string.
    
    Syntax

```cpp
str.replace(pos, count, newString);
```

Example

```cpp
string s = "Hello Java";
s.replace(6, 4, "C++");

cout << s; // Hello C++
```

- append()  
    Appends another string at the end.
    
    Syntax

```cpp
str.append(otherString);
```

Example

```cpp
string s = "Hello";
s.append(" World");

cout << s; // Hello World
```

- compare()  
    Compares two strings lexicographically.
    
    Syntax

```cpp
str.compare(otherString);
```

Example

```cpp
string a = "Apple";
string b = "Banana";

if (a.compare(b) < 0) {
    cout << "Apple comes first";
}
```

- c_str()  
    Returns a C-style character array.
    
    Syntax

```cpp
str.c_str();
```

Example

```cpp
string s = "Hello";
const char* ptr = s.c_str();

cout << ptr;
```

---

##### Iterators
- begin()  
    Returns an iterator to the first character.

```cpp
str.begin();
```

Example

```cpp
string s = "Hello";

for (auto it = s.begin(); it != s.end(); it++) {
    cout << *it;
}
```

- end()  
    Returns an iterator pointing just after the last character.

```cpp
str.end();
```

- rbegin()  
    Returns a reverse iterator to the last character.

```cpp
str.rbegin();
```

Example

```cpp
string s = "Hello";

for (auto it = s.rbegin(); it != s.rend(); it++) {
    cout << *it;
}
```

---

##### Less Common (But Useful) Functions
- capacity()  
    Returns the current allocated storage capacity.
    
- resize()  
    Changes the size of the string.
    
- swap()  
    Swaps the contents of two strings.
    
- copy()  
    Copies characters into a character array.
    
- data()  
    Returns a pointer to the underlying character array.
    
- starts_with() (C++20)  
    Checks if a string starts with a given prefix.
    
- ends_with() (C++20)  
    Checks if a string ends with a given suffix.
    
- getline()  
    Reads an entire line including spaces.
    

Example

```cpp
string s;
getline(cin, s);
```

---

##### Common String Operations

```cpp
string s1 = "Hello";
string s2 = "World";

// Concatenation
string s3 = s1 + " " + s2;

// Access Character
cout << s1[0]; // H

// Modify Character
s1[0] = 'h';

// Check Equality
if(s1 == s2) {
    cout << "Equal";
}

// Reverse String
reverse(s1.begin(), s1.end());

// Sort Characters
sort(s1.begin(), s1.end());
```