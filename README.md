# Let's break down the code !

1. main function:

* It checks the first command-line argument to determine whether it should run as the "parent" (creating the container) or the "child" (inside the container).
* If it's "run", it calls the parent function, otherwise, if it's "child", it calls the child function.
* If the argument is not recognized, it panics with an error message.

2. parent function:

* It creates a new command using /proc/self/exe to execute the same binary (program) with the "child" argument and the remaining arguments.
* It sets up the process attributes (SysProcAttr) with the required namespaces (UTS, PID, and Mount) using the Cloneflags field.
* It redirects the standard input, output, and error streams to the new command.
* It runs the command and handles any errors that might occur.

3. child function:

* It mounts the "rootfs" directory (root filesystem) to itself using the syscall.Mount function.
* It creates a new directory "oldrootfs" inside "rootfs".
* It calls the syscall.PivotRoot function to switch the new root filesystem to "rootfs" and move the old root filesystem to "rootfs/oldrootfs".
* It changes the current working directory to the new root ("/").
* It creates a new command using the provided arguments and redirects the standard input, output, and error streams.
* It runs the command and handles any errors that might occur.

4. must function:

A simple utility function that panics if the given error is not nil. It's used to simplify error handling in the child function.
To run this code, you need a root filesystem directory named "rootfs" that contains a minimal Linux distribution. When you run the program with the "run" argument, followed by a command, it will create a new process with separate namespaces and execute the given command inside the containerized environment.