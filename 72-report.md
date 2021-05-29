# 72-Report xv6 riscv-Rust
## Result analysis:
Our expected goal is to implement three system calls: sys_fork, sys_echo and sys_exec, and to implement a shell.

Eventually we have completed four aspects of sys_fork, sys_exec, file system and console.

"Sys_fork" and "Console" have been achieved. 

What still needs to be added are exec, echo, and the shell.

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

### 2. Console Read
### 3. Sys_Exec
### 4. File System
## Future direction:
## Summary:
## Division of labor:
