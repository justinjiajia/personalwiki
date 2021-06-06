


## The Startup Sequence

When a Unix machine comes to life, a particular sequence of events takes place. It is of interest to examine this sequence of events as follows:

1. machine power is turned on, a pre-determined address/command in ROM gets loaded into RAM and begins execution of the boot loader program


2. the loader process loads the system kernel program, starts the kernel running and passes control to it (loader process dies). On some Unix implementations, the kernel is simply named Unix, while in Linux, the kernel is named *vmlinuz*.


3. loaded kernel checks memory, checks and sets up device drivers, organizes memory, sets up file system, etc. It also begins to spawn processes, the most notable of these is the *init* process which has PID 1. The *init* program is responsible for creation of all subsequent processes and therefore is the (grand) parent of all processes. See the *init* man page.


4. the *init* process reads the */etc/inittab* file and performs two primary tasks
    1) sets the system states for all process execution (i.e. run levels) and
    2) starts a *getty* program running at each terminal port. *getty* is essentially a device driver. The *getty* process displays the

    > login:  

    prompt at its assigned terminal and wait for someone to type in something. The *getty* process then waits (sleeps) for a user to enter their username.


5. as a user enters begins to enter a username, the *getty* process wakes up and is overwritten (exec’d) by the *login* program, which then begins execution. The *login* process displays the

>	Password:  

prompt. The *login* process verifies the username-password pair entered against a corresponding user record in the */etc/passwd* file. If the username/password matches the password data in the */etc/passwd* file, the startup application (as specified in the */etc/passwd* file) is exec'd 1; if the username/password entry does not match, an error is given and access is denied.
if a Unix shell is the specified startup application 2, the specific shell will read and execute the configuration program native to that shell. For example, ksh executes .profile, csh executes .cshrc, and bash executes .bash_profile and/or .bashrc 3
A diagram of the described boot process follows, showing subtle differences for System V flavors vs. BSD varieties. Note that this is a generic startup sequence and may vary from Unix system to system. This also implies that documented file names may change somewhat from system to system (e.g. in Linux, the init program lives in /sbin/init) 4.


1 This implies that the login process is overwritten by the startup application. While this is true in some implementations, in Linux, the login process forks the startup application and remains alive (but asleep).
2 The startup application is typically a Unix shell, but it does not have to be, it could be another application program.

3 Numerous files end with the letters "rc" (e.g. .bashrc) on Unix installations. The letters rc are historical in nature and are taken from the words run commands to indicate the intended purpose of the file.

4 Since the original publication of this e-text, the init process has been replaced in some Linux distributions by the systemd process, also having PID 1. This has become somewhat controversial, and has been described as a violation of the Unix philosophy. More on the systemd process here. [Wikipedia contributors]





 If you connect via a program like ssh, you’ll be assigned a pseudo-terminal or pseudo-tty, in Unix parlance. That’s why when you typed in who you saw entries like ptty3 or pty1. In both instances, there is the program that reads your account and password information, and the program that validates it and invokes whatever login programs are needed for you to “log in” if everything checks out and is correct. As soon as someone types in some characters followed by the Enter key, the login program finishes the process of logging in



 ## Typing Commands to the Shell

 When the shell starts up, it displays a command prompt—typically a dollar sign `$` — at your terminal and then waits for you to type in a command (Figure 2.6, Steps 1 and 2).

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/command_cycle.PNG)

 Each time you type in a command and press the Enter key (Step 3), the shell analyzes what you typed and proceeds to carry out your request (Step 4).

If you ask the shell to invoke a particular program, it searches the disk, stepping through all the directories you’ve specified in your *PATH* until it finds the named program.

When the program is found, the shell creates a clone of itself (known as a subshell) and asks the kernel to replace the subshell with the specified program; then the login shell “goes to sleep” until the program has finished (Step 5).

The kernel copies the specified program into memory and begins its execution. This copied program is called a process; in this way, the distinction is made between a program that is kept in a file on the disk and a process that is in memory and being executed, line by line. If the program writes output to standard output, that output will appear at your terminal unless redirected or piped into another command. Similarly, if the program reads input from standard input, it will wait for you to type in that input unless redirected from a file or piped from another command (Step 6). When the command finishes execution, it vanishes and control once again returns to the login shell, which prompts for your next command (Steps 7 and 8).
