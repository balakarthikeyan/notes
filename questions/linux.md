1	What is Linux?	
Linux is a free and open-source operating system based on the Unix operating system. It was first created by Linus Torvalds in 1991 and has since become one of the most popular operating systems for servers, supercomputers, mobile devices, and embedded systems.

2	What is the difference between Linux and Unix?	
The main difference between Linux and Unix is that Linux is open-source software, while Unix is a proprietary operating system. Linux is based on the Unix operating system and shares many similarities, but has evolved separately and has its own unique features and tools.

3	What is a shell in Linux?	
A shell is a command-line interface used to interact with the Linux operating system. It allows users to enter commands and execute programs, and provides a way to customize the behavior of the system. Some popular Linux shells include bash, zsh, and fish.

4	What is the root user in Linux?	
The root user in Linux is the superuser account that has access to all commands and files on the system. The root user can perform any operation on the system, including creating, modifying, and deleting files and directories, installing and removing software, and managing system settings.

5	What is a Linux kernel?	
The Linux kernel is the core of the Linux operating system. It is responsible for managing system resources, such as the CPU, memory, and input/output devices, and provides an interface between the hardware and software components of the system. The kernel is open-source software and can be customized and modified by developers and users.

12	What is the difference between a soft link and a hard link in Linux?	
In Linux, a soft link (or symbolic link) is a file that points to another file or directory, while a hard link is a reference to a file that shares the same inode as the original file. Soft links can point to files or directories on different file systems, while hard links must be on the same file system as the original file.

13	What is a Linux process?	
A process in Linux is an instance of a running program. It is a unit of execution that has its own memory space, system resources, and priority. Processes can be started and stopped by the user, and can communicate with each other through interprocess communication (IPC) mechanisms such as pipes and sockets.

Linux operating systems, Whenever we are executing a command, we are creating three file descriptors :

- STDIN : also called the standard input that will be used in order to type and submit your commands (for example a keyboard, a terminal etc..);
- STDOUT : called the standard output, this is where the process outputs will be written (the terminal itself, a file, a database etc..);
- STDERR : called the standard error, it is very related to the standard output and is used in order to display errors.