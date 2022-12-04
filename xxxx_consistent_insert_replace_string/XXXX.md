---
title: Consistently overload `replace` and `insert` in `std::basic_string`
document: XXXX
date: today
audience: LEWG
author:
  - name: Kilian Henneberger
    email: <kilis-mail@web.de>
toc: false
---

[//]: # (https://eel.is/c++draft/basic.string.general)


# Introduction
`std::basic_string` has multiple overloads of the member functions `insert` and `replace`.
Every overload of `insert` takes as a first parameter the location to insert characters at.
The location can either be an index of type `std::basic_string<charT, traits, Allocator>::size_type` (hereafter referred to as `size_type`)
or an iterator of type `std::basic_string<charT, traits, Allocator>::const_iterator` (hereafter referred to as `const_iterator`).
After this first parameter follow one or multiple parameters which describe the characters that we want to insert.
Similarly, the overloads of `replace` take as first two parameters the range of existing characters, that we want to replace.
These first two parameters can either be an index and a length, each of type `size_type`, or two iterators of type `const_iterator`.
After these first two parameters follow one or multiple parameters which describe the new characters that we want to use as replacement.
The following table shows an overview of the available overloads of `insert` and `replace`.

| Overloads |     `insert(size_type, ...)`     | `insert(const_iterator, ...)` |     `replace(size_type, ...)`     | `replace(const_iterator, ...)` |
|-----------|:-------------:|:-:|:-:|:-:|
| `const basic_string& str` | Yes |  |  Yes | Yes |
| `const basic_string& str, size_type pos2, size_type n = npos` | Yes | |Yes  |    |
| `const StringViewish& sv` | Yes | | Yes |  Yes  |
| `const StringViewish& sv, size_type pos2, size_type n = npos` | Yes | | Yes |    |
| `const charT* s, size_type n` | Yes  |   | Yes  | Yes  |
| `const charT* s` | Yes  |   | Yes  |  Yes |
| `size_type n, charT c` | Yes  | Yes   | Yes  |  Yes  |
| `charT c` |  | Yes   |  |    |
| `InputIterator first, InputIterator last` |  | Yes   |  | Yes   |
| `initializer_list<charT> il` |  | Yes  |  | Yes  |


Surprisingly, the overloads of `insert` taking a `size_type` as first parameter are not consistent with the overloads, taking a `const_iterator` as first argument.
Hence, the following code will not compile.
```cpp
std::string s1;
std::string s2;
s1.insert(0, s2); // compiles
s1.insert(s1.begin(), s2); // does not compile
```

TODO: Und nun? Alle fehlenden overloads hinzufügen? Oder nur die, die auf iteratoren als erste Argumente arbeiten?


## `std::string::insert_range` & `std::string::replace_with_range` (added by [P1206](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p1206r7.pdf))
Only overloaded with `const_iterator`(s) as first argument(s)



# `StringViewish` vs `std::baisc_string_view`
TODO

# Wording
TODO

## Feature-test macro
Yes or no?

# Acknowledgments

# References