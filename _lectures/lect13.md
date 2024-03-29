---
num: "Lecture 13"
desc: "Basic OS Concepts"
ready: true
lecture_date: 2019-11-12 14:00:00.00-7:00
---

## Operating Systems
* "A program that facilitates easy, efficient, fair, orderly, and secure use of various software and hardware resources"
* Can be viewed as a resource manager
* Can manage and share hardware resources with multiple applications
* Operating Systems manage hardware such as
	* mouse, keyboard, monitor, hard disk, RAM, Network Interface Card (NIC), CPU, printer...
* Operating Systems provides an interface for programmers to use in the form of libraries / system calls.
	* <b>OS kernel</b> is the "core" of the OS that manages resources.
	* Exists in <b>kernel space</b>, which is separate from user-space where application memory exists.

## A (simplified) view of the Application / OS / Hardware Stack.

* <b>Application</b>
	* vim, emacs, cpp executables, .py scripts, UNIX shell, ...
	* Application Programming Interface (API): 
		* Contains executable code for OS functionality and language libraries
		* Language Libraries (C, C++, Java, Python, …)
		* Language libraries can use system calls.
			* For example std::cout (C++), System.out (Java), print (Python).
* <b>Operating System (OS) Kernel</b>
	* Memory space that contains functionality the OS provides to languages and applications.
	* File Management functionality, CPU scheduling, Process Management, IO, Memory Management (RAM and HD).
* <b>Hardware</b>
	* Physical components of a computer (anything you can physically touch).
	* Interfaces with the OS using device drivers (software operating the hardware device).

## Processes
* A computer program in execution
* Or an instance of a program being run on the computer
* Contains the memory and state of the program being run

## Threads
* A program unit that is executed independently of other parts of your program.
* A process may create threads
	* An OS manages threads similar to a process
	* But a thread shares the memory space of the main process.

## Process Status (ps) command
* Can view active processes on your terminal with the `ps` command.
	* `ps -l` provides more details on active processes

```
MacBook-Pro-38:lecture Richert$ ps -l
  UID   PID  PPID        F CPU PRI NI       SZ    RSS WCHAN     S             ADDR TTY           TIME CMD
  501 31029 31027     4006   0  31  0  2498932    492 -      R+                  0 ttys000   16:29.05 -bash
  501 68157 68156     4006   0  31  0  2489716   3428 -      S+                  0 ttys001    0:00.14 -bash
  501 68585 68584     4006   0  31  0  2489716   3368 -      S                   0 ttys002    0:00.07 -bash
```

* In this example, three terminal processes are currently executed.
* Each terminal window (`bash`) has a unique process ID (`PID`)
* Each terminal window has a parent process (`PPID`) that created this process.
* Note, the `ps` command only shows the processes from the terminal (not the entire OS).
	* You can use `ps -e` or `ps -A` to view the entire list of processes active on your OS.

## `top` command
* Processes consume CPU and memory resources when they're being executed.
* `top` shows the resource consumption of the top-most consuming processes.
* Example:

```
Processes: 395 total, 4 running, 391 sleeping, 2436 threads                                                          19:35:58
Load Avg: 1.92, 1.73, 1.65  CPU usage: 6.82% user, 15.52% sys, 77.64% idle
SharedLibs: 156M resident, 38M data, 37M linkedit. MemRegions: 215376 total, 6562M resident, 110M private, 1222M shared.
PhysMem: 16G used (4285M wired), 79M unused.
VM: 1096G vsize, 627M framework vsize, 1454320269(0) swapins, 1468765567(0) swapouts.
Networks: packets: 97637475/70G in, 65969671/12G out. Disks: 64971993/6343G read, 61258168/6352G written.

PID    COMMAND      %CPU TIME     #TH   #WQ  #PORTS MEM    PURG   CMPRS  PGRP  PPID  STATE    BOOSTS              %CPU_ME
97722  Google Chrom 0.0  00:32.88 18    2    138    5188K  0B     55M    47766 47766 sleeping *0[1]               0.00000
97702  Google Chrom 0.0  00:26.14 17    1    143    4988K  0B     43M    47766 47766 sleeping *0[1]               0.00000
97678  Google Chrom 0.0  00:40.43 18    2    138    3644K  0B     53M    47766 47766 sleeping *0[1]               0.00000
97675  Google Chrom 0.0  00:32.14 17    1    137    3428K  0B     55M    47766 47766 sleeping *0[1]               0.00000
97674  Google Chrom 0.0  00:30.36 9     1    94     8012K  0B     66M    47766 47766 sleeping *0[1]               0.00000
97673  Google Chrom 0.0  30:11.52 16    1    155    49M    0B     93M    47766 47766 sleeping *0[1]               0.00000
97670  Google Chrom 0.0  00:22.85 17    1    143    5192K  0B     41M    47766 47766 sleeping *0[1]               0.00000
97528  MTLCompilerS 0.0  00:00.73 2     2    31     120K   0B     6920K  97528 1     sleeping  0[4]               0.00000
97023  Slack Helper 0.0  44:29.52 5     2    139    50M    2608K  220M   2308  2308  sleeping *0[1]               0.00000
94576  com.apple.ap 0.0  29:25.52 3     1    394    32M    0B     22M    94576 1     sleeping *0[100448]          0.00000
94575  TextEdit     0.0  22:45.46 6     1    523    33M    0B     56M    94575 1     sleeping *0[59144]           0.00000
91340- dsAccessServ 0.0  00:14.24 5     1    57     1052K  0B     2324K  91327 91327 sleeping *0[1]               0.00000
91327- dsAccessServ 0.0  28:13.91 13    4    119    3504K  0B     3484K  91327 1     sleeping *0[1]               0.00000
91232- PulseTray    0.0  02:57.28 6     1    252    8892K  0B     26M    91232 1     sleeping *0[1]               0.00000
90135  familycircle 0.0  00:03.11 2     2    47     232K   0B     2644K  90135 1     sleeping  0[25]              0.00000
87331  Google Chrom 0.0  14:05.59 19    2    159    86M    0B     204M   47766 47766 sleeping *0[1]               0.00000
```

## Example of creating our own process

```
# Makefile
CXX=g++
main: main.o
	${CXX} -o main -std=C++11 main.o
clean:
	rm -f *.o main
-----
// main.cpp
#include <unistd.h>
using namespace std;
int main() {
	while (true) { sleep(10000); }
}
```

## Foreground / Background Processes
* You can run a program in the foreground (the command waits for the program to terminate before the terminal accepts more commands)
* We’ve done this all quarter long by executing a program (ex: `./main`)
* Executing a process with & runs it in the background
* `jobs` command lists all processes running in the background
* Example

```
MacBook-Pro-38:lecture Richert$ make main
g++    -c -o main.o main.cpp
g++ -o main -std=C++11 main.o
MacBook-Pro-38:lecture Richert$ ./main
^C
MacBook-Pro-38:lecture Richert$ ./main&
[1] 68713
MacBook-Pro-38:lecture Richert$ ps -l
  UID   PID  PPID        F CPU PRI NI       SZ    RSS WCHAN     S             ADDR TTY           TIME CMD
  501 31029 31027     4006   0  31  0  2498932    492 -      R+                  0 ttys000   24:33.59 -bash
  501 68157 68156     4006   0  31  0  2489716   3428 -      S+                  0 ttys001    0:00.14 -bash
  501 68585 68584     4006   0  31  0  2489716   3380 -      S                   0 ttys002    0:00.10 -bash
  501 68713 68585     4006   0  31  0  2433800    664 -      R                   0 ttys002    0:01.72 ./main
MacBook-Pro-38:lecture Richert$ jobs
MacBook-Pro-38:lecture Richert$ ./main&
[1] 68726
MacBook-Pro-38:lecture Richert$ jobs
[1]+  Running                 ./main &
MacBook-Pro-38:lecture Richert$ ps
  PID TTY           TIME CMD
31029 ttys000   25:56.74 -bash
68157 ttys001    0:00.14 -bash
68585 ttys002    0:00.12 -bash
68726 ttys002    0:06.81 ./main
MacBook-Pro-38:lecture Richert$ kill 68726
[1]+  Terminated: 15          ./main
```

## Suspend / Resume processes
* You can suspend a running process in the foreground (not terminate it) with `<cntrl z>`
* Any program that the terminal is running can be run on the foreground or background using the `fg` or `bg` commands
* Note that ^C is not the same as ^Z
	* `<cntrl C>` terminates the process. `<cntrl-Z>` stops execution, but the process still exists.

```
MacBook-Pro-38:lecture Richert$ ./main
^Z
[1]+  Stopped                 ./main
MacBook-Pro-38:lecture Richert$ jobs
[1]+  Stopped                 ./main
MacBook-Pro-38:lecture Richert$ bg %1
[1]+ ./main &
MacBook-Pro-38:lecture Richert$ jobs
[1]+  Running                 ./main &
MacBook-Pro-38:lecture Richert$ ps
  PID TTY           TIME CMD
31029 ttys000   33:08.34 -bash
68157 ttys001    0:00.16 -bash
68768 ttys001    0:06.58 ./main
MacBook-Pro-38:lecture Richert$ kill 68768
[1]+  Terminated: 15          ./main
```
