## Welcome!

This is Assignment 1 for CSCB09, the Systms Monitoring Tool. This project is a simple tool that displays the CPU and memory usage of a system, as well as other information.

## Running the files

1. To run the files, you will need to compile the code in your compiler of choice:
    [compiler of choice] mySystemStats.c
2. Then, you will need to run the executable file:
    ./a.out

    The program also takes optional command arguments:

   --system: displays the system stats (memory and CPU data) only

   --user: displays the user and session stats only

   --graphics: adds a graphic display of the memory and CPU usage

   --sequential: runs the program in sequential mode where the program will run interactions, perfect to output to another file

   --samples=N: sets the number of samples to N (default is 10) 
        can also be activated by writing only a number after the program name, e.g. ./a.out 5

   --tdelay=T: sets the delay between samples to T seconds (default is 1) 
        can also be activated by writing only a number after the program name, but must also include a sample value e.g. ./a.out 5 2

4. The program will then run and display the system stats.

## Table of contents
<!-- 
- [Overview](#overview)
  - [Built with](#built-with)
  - [Function insights](#function-insights)
  - [Continued development](#continued-development)
- [Author](#author) -->

## Overview

### Built with
I built this with C, using the following libraries:
- stdio.h
- stdlib.h
- string.h
- sys/resource.h
- sys/utsname.h
- sys/sysinfo.h
- sys/types.h
- utmp.h
- unistd.h
- ctype.h 
- math.h

### Function Insight
main() - The main function of the program. It takes in command line arguments and runs the program accordingly. It also calls the other functions to display the system stats. It stores integers that represent if the command arguments have been activated and then runs the program accordingly. It also stores the number of samples and the time delay between samples. It then runs the program in a loop depending on the number of samples + 1, calling the other functions to display the system stats. Every time the loop iterates, the program sleeps before it clears the screen using ANSI escape codes and displays the updated stats. During the first iteration of the loop, the program also stores the initial CPU values to use for the CPU and memory usage calculations and begins displaying the memory and CPU values on the next iteration so that the program can compare the current CPU values to the initial CPU values. This is why the loop runs for the number of samples + 1.

print_memory_usage() - This function prints the memory usage of the system in KB. It uses rusage from the sys/resource.h library to get the maximum memory usage and then prints it to the console. The maximum represents the maximum memory the program can use.

print_memory_info() - This function prints the memory information of the system. It uses sysinfo from the sys/sysinfo.h library to get the memory information and then prints it to the console. The memory information includes the used physical memory, total physical memory, free virtual memory, and total virtual memory in GB. The physical memory is the actual RAM of the system, while the virtual memory is the combination of the RAM and the swap space. Therefore, to get the used physical memory, the function subtracts the free virtual memory and the buffer from the total virtual memory. This was done to get a value that is closer to the actual used physical memory. The used virtual memory is very similar, obtained by subtracting the free virtual memory and buffer from the total virtual memory.
This function takes in an array that stores the memory information and updates it every iteration to create buffer space for easier reading when the program clears the screen and displays the updated memory information.
If the graphics flag is activated, the function prints '####* ' for positive changes in the used virtual memory, '::::@' for negative changes, '@' for -zero values, '*' for +zero values, and 'o' for a true zero. This is done to create a visual representation of the memory usage. The number of '#' or ':' depends on the decimal values of the used virtual memory difference. The changes in the used virtual memory are calculated by comparing the current used virtual memory to the previously used virtual memory, before rounding, meaning that values such as 0.0005 get represented as 0, creating an illusion of no change. This is why there is a distinction between +zero, -zero, and true zero values. The first displayed value, for example, we know is a true zero since it is the first, and will display 'o'.
If the sequential flag is activated, the function only displays the current memory information, emptying the memory array of the previous values.

print_users_usage() - This function prints the users and session information. It uses utmp from the utmp.h library to get the users and session information and then prints it to the console. The user and session information includes the user name, line information, and hostname. The user name is the name of the user, the line information is tty or pts (the way a user is logged on), and the hostname is the name of the host. To get this information, the function uses getutent() to get the user information by iterating through the utmp file. 

print_cpu_usage() - This function prints the CPU usage of the system. It shows the number of processors running by calling sysconf(_SC_NPROCESSORS_ONLN), which shows all processors online. It uses the system information from /proc/stat to get the CPU usage and then prints it to the console. The CPU usage is calculated in the get_cpu_info() function. After it obtains this information, it calculates the CPU usage by taking the work, the difference between the current and previous values of the first three numbers in proc (user, nice, and system). It then takes the total, the difference between the current and previous totals of all the /proc/stat/ values (user, nice, system, idle, iowait, irq, and softirq). The final CPU usage is the (work/total)*100 to get the value as a percentage. The CPU usage is then printed to the console. The difference between the current CPU values and the previous is always the time the program has been running apart. The program takes an initial CPU reading at the program start and compares it to all future CPU values, meaning that the final CPU total displayed compares the program end values to the program start values. 
If the graphics flag is activated, the function prints '|||' to represent the CPU usage. The number of '|' depends on the decimal values of the CPU usage. The number of '|' is calculated by taking the floor of the CPU usage + 3.
If the sequential flag is activated, the function only displays the current CPU information, emptying the cpuarray of the previous values.

get_cpu_usage() - This function reads the first line of /proc/stat to get the CPU usage. It then parses the line to get the values of the user, nice, system, idle, iowait, irq, and softirq. It then calculates the total of the first three values (user, nice, and system) and the total of all the values. It then returns the total and the work (the first three values).

print_system_info() - This function prints the system information of the system. It uses uname from the sys/utsname.h library to get the system information and then prints it to the console. The system information includes the system name, machine name, version, release, architecture, and time the system has been running. The system name is the name of the operating system, the machine is the node name, the version is the release version, the release is the release date, the architecture is the machine information, and the time is the time the system has been running, shown in a few different ways.

### Continued development

After finishing this project, I am looking forward to the next assigments and the challenges they may bring. 

## Author

- LinkedIn - [Katherine Pravdin](https://www.linkedin.com/in/katherinepravdin)
- GitHub - [Kalshone](https://www.github.com/kalshone/)



