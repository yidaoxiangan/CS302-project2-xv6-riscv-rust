# 72-Report xv6 riscv-Rust
## Result analysis:
Our expected goal is to implement three system calls and a shell: "sys_fork", "sys_echo" and "sys_exec", and a "shell".

Eventually we have completed four aspects of "sys_fork", "sys_exec", "file system" and "console". Because "file system" and "console" are necessary for "shell" and "sys_echo"

"Sys_fork" and "Console" have been achieved. 

What still needs to be improved are "sys_exec", "sys_echo", and the "shell".

Due to the serious lack of functions in the whole project, we need to make up a lot of unexpected code.

### 1. Sys_Fork
<!-- ![](./images/fork.png) -->
<img src="./images/fork.png"  height="550" width="550">

Because we haven't implemented the complete sys_exec and file system, we can't enter the user mode.

So we test "Sys_Fork" separately in kernel mode, and the program run fork until it can no longer allocate a new processes.

The maximum number of processes in xv6 is 64, and the initialization process is called "0", so we can fork 63 processes.

### 2. Console Read(shell need it)
<div align="center">
<img src="./images/console1.png"  height="300" width="400">
<img src="./images/console2.png"  height="300" width="400">
</div>

Because in the original project, users can't input characters in the console.

The implementation "shell" must interact with the user, so we need to implement the input of the console.

The maximum size of input buffer of console is 128, and the input of 128 characters is supported at one time.


### 3. Sys_Exec

It is divided into two parts. One is to use path to find inode, one is to load elf file.

At present, we complete the first part: finding the inode of the file by using path.

Since the later part needs the support of file system, we need to repair the file system.

### 4. File System(shell and sys_exec need it)


## Implementation:
### 1. Sys_Fork

Preparation steps: "Sys_fork" is a function in the "Proc.systemcall" structure. If it want to be called in user mode, we need to define its macro as Sys_fork = 1.

First step: We need to allocate a new process "A" from PROC_MANAGER. PROC_MANAGER will give a pid, procstate, pagetable, context and so on. In the allocation process, we need to lock to avoid asynchronous competition for "A".

Second step, UVM Copy: Given a parent process's page table, copy its memory into a child's page table. Copies both the page table and the physical memory. So we need to find physical memory by using virtual address and pagetable.

Final step: We will give "A" some parameters different from the original process such as: pid, cwd, refernce counts and so on. At the same time, do not forget to lock when modifying the parameters in the "A" process.

Fork code:

![](./images/fork_code.png)

UVM Copy code:

![](./images/uvm_code.png)

### 2. Console Read

Background: Because we want to implement the shell, users must be able to communicate with the program through the console. But in the original project, the console can't let users input, so we need to implement an interactive function to make user can input through the console.

First step: First we need to understand what happens to the program after the user enters some letters into the console. The answer is that the program will be interrupted by "UART". Therefore, we need to add our console processing code to the UART interrupt processing code.

Second step: When we enter the console code, we need to get data from register LSR to determine whether it is a write operation. Then we get the data of type U8 from register RHR and print it out to let the user know that it is input correctly. All input characters are stored in the console buffer with a maximum size of 128.

Final step: Then we need to transfer all the characters in the console buffer to the user mode read buffer. In this way, the user-mode code and users can read the input from the kernel.

Interrupt handle code:

![](./images/trap.png)

Get data from register code:

![](./images/register.png)

Handle input data in console code:

![](./images/console.png)

### 3. Sys_Exec

Background: Any program stored in user mode must be started through "exec". But the essence of "exec" is to run a compiled executable file. In RISC V, it is an ELF file. Therefore, the first need to have a file system to store files, and the second is to load the elf file into memory.

First step: First, we get a path, we need to get his file inode through the path. Then load the ELF file in the storage space corresponding to inode.

Second step: But "exec" needs to use the file system, but the file system implementation in the original project is not complete, there are many things we need to add. So we're going to improve the file system.

Get inode from path code(Incomplete, file system required):

![](./images/inode.png)

### 4. File System
## Future direction:
## Summary:
## Division of labor:
