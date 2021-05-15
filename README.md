# Session 2 assignment of EPAi3.0

The session 2 of EPAi3 is about Object mutability and Interning.

## Class Notes Summary

This document gives a brief overview of things we learnt in the session.

#### 1. Variables are memory references

Each variable is a mere pointer to the memory reference.

```angular2html
my_var = 10
print(f'my_var = {hex(id(my_var))}')
```

Output:

```angular2html
my_var = 0x7fa5d744e560
```

#### 2. Object Interning

It is the reuse of objects (memory references) instead of creating new objects every time. This section talks about how
the standard implementation of python uses interning to optimize the variables assignment for memory management.

* For integers, python pre-loads -5 to +256.
* For Strings, interning happens only for 'valid identifier' i.e. string that contains only alphanumeric letters and
  underscores and string should not be 20 chars long. One can force the object to be intern using `sys.intern()`.

```angular2html
import sys

a = sys.intern("String not intern")
b = sys.intern("String not intern")

print({hex(id(a))})
print({hex(id(b))})
```

Output

```angular2html
var a = 0x7fa33e7562b0
var b = 0x7fa33e7562b0
```

**Where to use interning?**

From [reference](https://levelup.gitconnected.com/optimization-in-python-the-interning-technique-for-improved-performance-3ff14d376176)
,
*we should be very reasonable while using interning. For example, we can use it in NLP where we have to deal with a
corpus of textual data having a large number of strings that could have high repetition. When we tokenize a large set of
text data, we get a high of occurrence of words like — ‘a’, ‘the’, ‘is’. For instance, let’s assume we got 10000
repetitions of the word ‘the’. Instead of creating 10000 objects for each of the 10000 repetitions, we can use interning
to create a singleton object and then each and every occurrence of ‘the’ will point to same memory location. In that
case, we can use interning to optimize the code performance*.

#### 3. Reference Counting

We can count the number of references to a variable using ctypes. This can be used for memory profiling.

```angular2html
ctypes.c_long.from_address(address).value
```

#### 4. Cyclic Reference

There can be cyclic object references and new version python's gc is intelligent to clean them up.

#### 5. Dynamic Typing

Type of the variable is the type in the memory address the variable is pointing to. Variables or references do not have
any intrisinc type. The object to which a reference is bound at a given time defines the type of the variable. There are
also no type declarations.

#### 6. Object Mutability

Some functions of objects are mutable and others are not. Example for a list, append operation mutates the object while
'+' creates a new list.

#### 7. Every object is an instance of a class

Every variable is an instance of a class. The memory usage of the variable includes the references to the methods in the
class. Even a method reference is an instance of class Function.

#### 8. Variant Equality

Python will attempt to compare the values as best as possible.

## Assignment

The assignment contains some test cases that fails. We will have to fix the code to make sure the tests pass.

* Key classes in `session2.py`
    * class `Something` and `SomethingNew` demonstrates the use of cyclic memory reference. As a part of `__init__`
      instance of class `SomethingNew` is created.

* Key functions in `session2.py`
    * `add_something` creates an instance of class `Something` and adds it to a collection
    * `clear_memory` clears the collection and we have added a patch to force perform garbage collection to actually
      cleanup the memory.
    * `critical_function` creates a collection of `Something` object and clears the memory.
    * `compare_strings_old` compares two strings in unoptimized way using `==` operator for string comparison and
      generating a `char_list` from the string to check if a character is present.
    * `compare_strings_new` new optimized implementation of comparing strings and checking if a character is present in
      a string. In this function, we have replaced the `sleep` implementation with the actual code for comparison.

  


