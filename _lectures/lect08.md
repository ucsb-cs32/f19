---
num: "Lecture 8"
desc: "Midterm 1 Review"
ready: true
lecture_date: 2019-10-22 14:00:00.00-7:00
---

```
Logistics
- Bring studentID and writing utensil
- No electronic devices
- No notes, no books

Format
- Will be a mix of questions based on lectures, homeworks, readings,
and labs
- Short answer
  - briefly define, describe, state, ...
- Write code (including stuff in makefiles)
- Fill in the blank / complete the table
- Given code, write the output
- Given some statement, temm me if it's true or false
  - If false, briefly explain why
- Expect it to take ~ one hour
  - but we have the entire class period
- Will cover a broad range of topics, but probably not everything

Topics
- Will cover everything up to Thursday's lecture (10/17)

Makefiles
- Know how to create one when multiple files are linked to form
an executable
- Know the build process
  - Preprocessor, Compilation, Linker
- Creating and using variables
- Know the characters like $@ and $^
- Know what default rules are, flags for compilation...

Standard Template Library
- Reviewed some details about vectors
  - Operations like [], at, front, back, push_back, size, constructors, ...
- STL iterators
  - std::vector, std::map, std::set (unordered_map, unordered_set)
  - .begin(), .end(), ++, <, *, erase
  - Using iterators in container methods as discussed in lecture

Class Design
- Abstract Data Types
- How to design an interface (.h) and its implementation (.cpp)
- Public vs. Private
- Accessors (getters) vs Mutators (setters)
- Constructors (default, overloaded, copy)
- Header guards
- Scope Resolution Operator (::) and .cpp method definitions
- Shallow vs. Deep Copy
- Big Three (Rule of Three): Copy constructor, destructor, assignment operator

Structs and Classes
- What are the difference(s)
- Memory padding (how are these stored in memory, what's the size, what's the benefit)

Namespaces
- Avoid naming collisions
- How to create namespaces in your code
- Global namespace

Quadratic Sorting Algorithms
- bubblesort (optimized version)
- selectionsort
- insertionsort
- Know the algorithms as discussed in lecture
- Know the running times

Hash Table Topics
- Know the basics of hash functions and performance of various operations
- open-address hashing / double-hashing / chained-hashing
- std::map vs std::unordered_map
    - Note, that hash tables are used when implementing sets, but sets do not have a
    corresponding value for each key. (std::set, std::unordered_set)

Quicksort / Mergesort
- Know the implementation and runtime analysis for various scenarios
```