---
num: "Lecture 1"
desc: "Introduction, Compilation Review"
ready: true
lecture_date: 2019-09-26 14:00:00.00-7:00
---

# Course Syllabus

Be sure to read the course syllabus for information on the logistics of this course.

<https://ucsb-cs32.github.io/f19/info/syllabus/>

# Academic Integrity

Take a minute to read UCSB’s Academic Integrity Policy. All students are responsible for understanding UCSB's academic integrity policy: <http://judicialaffairs.sa.ucsb.edu/academic-integrity>

# Makefiles (a simple example)

```
// main.cpp
#include <iostream>

using namespace std;

int main() {
	cout << "Hello CS 32!" << endl;
	return 0;
}
```

## Can compile this with g++ or clang++:

```
$ g++ main.cpp
$ ls
a.out		main.cpp
$ ./a.out
Hello CS 32!
```

* Note that the executable is called `a.out`. This is the default name of the executable.

## Using `make` command

```
$ make main
c++     main.cpp   -o main
```

* This default behavior of `make` tries to compile the .cpp with that name and output the executable to that name.
* Works well for very simple programs that exist in one file.

## Change the output of the executable with the `-o` flag
```
$ g++ -o main main.cpp
$ ls
functions.cpp	functions.h	main		main.cpp
$ ./main
Hello CS 32!
```
* Changes the executable name from `a.out` to `main`

# C++ Build Process

1. Preprocessor: Text-based program that runs before the compilation step. Looks for statements such as #include and modifies the source which is the input for compilation.

2. Compiler: A program that translates source code into “object code,” which is a lower-level representation optimized for executing instructions on the specific platform. Lower level representations are usually stored in a .o (object) file.

3. Linker: Resolves dependencies and maps appropriate functions located in various object files. The output of the linker is an executable file for the specific platform.

# Compiling multiple files example
```
--------
// functions.h

int doubleInt(int x);
--------
// functions.cpp

int doubleInt(int x) {
	return 2 * x;
}
--------
// main.cpp
#include <iostream>
#include “functions.h”

using namespace std;

int main() {
    cout << doubleInt(12) << endl;
    return 0;
}
--------
```

* We can't just compile main.cpp anymore...

```
$ g++ main.cpp
Undefined symbols for architecture x86_64:
  "doubleInt(int)", referenced from:
      _main in main-7c4a04.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

* we see a "linker" error.
* The linker doesn't know where to find the `doubleInt()` function definition.
* We need to compile `functions.cpp` so `main.cpp` knows where to find the `doubleInt()` function.

```
$ g++ -o main main.cpp functions.cpp
$ ls
functions.cpp	functions.h	main		main.cpp
$ ./main
24
```

* Makefiles are useful since writing commands with many files is tedious and error-prone.
* Makefiles are used to define compilation rules and dependencies so we can simply type `make [some_target]` and not have to type everything out all the time.

# General format of a Makefile
```
[target]: [dependencies]
	[commands]
```

* Example:

```
# Makefile

main: functions.cpp main.cpp
	g++ -o main main.cpp functions.cpp
```

```
$ make main
g++ -o main main.cpp functions.cpp
$ make main
make: `main' is up to date.
```

* In this case, `make` uses timestamps to determine if something needs to be recompiled.
* If a recent change occurred, then it will recompile. If nothing changes, then nothing is done since everything is `up to date`

## Note:
* The object (.o) files are not explicitly generated.
* You will need to use the `-c` (compile only) flag.
* Having make use .o files as a dependency is useful for maintenance reasons.
	* <b>Only</b> recompiles source files that have changed. 

```
# Makefile

main: functions.o main.o
	g++ -o main main.o functions.o
```
```
$ make main
c++    -c -o functions.o functions.cpp
c++    -c -o main.o main.cpp
g++ -o main main.o functions.o
$ ls
Makefile	functions.h	main		main.o
functions.cpp	functions.o	main.cpp
```

## Variables in Makefiles

```
# Makefile
#CXX=clang++
CXX=g++
DEPENDENCIES=functions.o main.o

main: ${DEPENDENCIES}
	${CXX} -o main ${DEPENDENCIES}
```

## make clean

* Useful when trying to remove the executable and all .o files so everything can be recompiled from scratch.

```
# Makefile
main: functions.o main.o
	g++ -o main main.o functions.o

clean:
	rm -f *.o main
```

* If we want to "start fresh" and recompile everything, it's nice to have a "clean" target to remove all object files and the executable

# Compiling a specific version of C++

* We can specify the specific C++ version to use when compiling our programs with a special std flag.

```
main: ${DEPENDENCIES}
	${CXX} -std=c++11 -o main ${DEPENDENCIES}
clean:
	rm -f *.o main
```

# Using `$@` and `$^`

* Obtaining the target and dependencies is a common pattern when writing Makefile rules.
* We can use `$@` and `$^` to obtain the target and dependencies respectively.
	* `$@` - target
	* `$^` - dependencies

* Example:

```
main: ${DEPENDENCIES}
	${CXX} -std=c++11 -o $@ $^

# Expands out to
# main: functions.o main.o
#	g++ -std=c++11 -o main functions.o main.o
```
