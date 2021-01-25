# Lab0


## 1. Inspect XINU's first process.

The code for the first user process that is created by XINU is located in system/main.c. By default it creates a single process that implements the XINU shell. Furthermore, it repeatedly creates a new shell after each shell process returns.

Processes in XINU are created using the create system call which does the following: (1) creates all necessary control structures for the new process and (2) initializes the process stack for the new process. The create system call returns the process identifier (PID) of the new process. The code for the create system call is located in system/create.c. Open system/create.c and familiarize does.

Modify system/main.c so that it prints the process ID of each new shell process by producing messages with the following format: "New shell PID = %d\n". Ensure the message is printed before the shell process starts running by invoking the kprintf() kernel function before the new process is resumed.

 </br>


-----------------------------------------------------

## 2. Create a new system call 

The XINU system calls are declared in the file include/prototypes.h. Create a system call named "hello" that receives no argument and always returns success (retval = 1 ). Implement the new system call inside a new file system/hello.c. The new system call should print the following message using the kprintf kernel function: "Hello system call invoked\n"

Remember to call $ make rebuild when adding new files to the Xinu source tree.

 </br>


-----------------------------------------------------

## 3. Inspect the shell code and implement a new shell command
    
The shell code is in the directory shell/. The shell/shell.c file contains the structure cmdtab with all the commands recognized by the shell that the user can invoke. Each field specifying a command contains the command name and the command function, in addition, it contains a bool field that specifies whether the command is built-in or not.

Create a new shell command called "hello". It should be implemented with a new function, xsh_hello, that you should define in a new file: shell/xsh_hello.c. Ensure that the new function is declared in the header file include/shprototypes.h. The definition of the function should call the hello system call that you implemented in 3.

After implementing this command, when the user types the "hello" command in the Xinu shell, it should cause the kernel to print the message: "Hello system call invoked\n".

 </br>


--------------------------------------------------------------


## 4. Memory layout 

Using the same procedure described in 4, create a new command called "layout", in a new file shell/xsh_layout.c, and implement it in a function called xsh_layout. When creating the command ensure that it is not a built-in command (which would create a new process function instead of executing a function).

The new command should print the address of: 
   
   * (1) an initialized global variable "int layout_var_init = 1;", 
   * (2) an uninitialized global variable "int layout_var_uninit;" 
   * (3) a stack variable "int layout_stack;".
   * (4) Furthermore, the command should print the address of the xsh_layout function.

 </br>
 
--------------------------------------------------------------

# XINU Labs

The XINU lab is located in Haas257. There are 21 Linux frontend machines xinu01.cs.purdue.edu, xinu02.cs.purdue.edu, xinu03..., ..., xinu21.cs.purdue.edu which are linked to 96 backend machines galileo101.cs.purdue.edu, galileo102.cs.purdue.edu, ..., galileo196.cs.purdue.edu.

The front machines served as operating system code development (coding, compiling, etc) while the backend machines served as actual operating system running platform and it would be either accessed physicly or remotely via TLS/SSL applications such as ssh on Linux/MacOS/Windows or PuTTY for some early version Windows.

The backend machines are x86 Intel Galileo boards equipped with Quark X1000 processors that are dedicated to running your implementation of XINU.

