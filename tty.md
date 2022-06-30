
references:

https://williams-cs.github.io/cs331-f19-www/assets/misc/pseudoterminals.html

https://www.linusakesson.net/programming/tty/

https://blog.bachi.net/?p=9034

https://dev.to/napicella/linux-terminals-tty-pty-and-shell-192e


https://developpaper.com/overview-of-linux-tty-pts-differences/

https://blog.csdn.net/leshami/article/details/77101939

https://www.howtogeek.com/727213/what-are-teletypes-and-why-were-they-used-with-computers/


TTY history

Before the advent of multitasking computers

Before computers came out, people had been using a device called teletype, which was used to transmit information to each other. It looked like this:



>+----------+   Physical Line        +----------+
>| teletype |<--------------------->| teletype |
>+----------+                       +----------+

Two teletypes are connected by wires. There may be devices like modems at both ends of the line (they are ignored here). When you type the keyboard on one end of the teletype, the corresponding data will be sent to the teletype at the other end. I don’t know what the specific functions are. (it occurred to me that the picture was typed at one end and printed at the other end.)

These are old antiques. I haven’t touched them at all, so I can only speculate simply.


什么是终端(Terminal)
早期的计算机都属于大中型计算机，是个庞然大物，占用很大的空间，属于公用产品，好比现在的共享单车，大家一起用。不像现在的电脑，可以人手一部，直接操作。那肿么办呢，如何对这些计算机进行控制与操作呢。那就搞个终端设备来操作。因此一台计算机上有很多种不同的终端设备也和正常。也就是说终端就是为主机提供了人机接口，每个人都通过终端使用主机的资源。终端有字符终端和图形终端两种。同时这些大型计算机还配有控制台。控制台是一种特殊的人机接口, 是人控制主机的第一人机接口。而主机对于控制台的信任度高于其他终端。控制台可以类比为我们操作系统的超级管理员，可以禁用某个用户的权限，禁用用户登陆等等。而普通终端就相当于一个普通用户



A terminal is a device for providing input to a program and printing output from the same program. The original terminals used paper! This device is called a teletype.


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/teletype.jpg)

At some point, “terminals” became CRT screens,

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/DEC_VT100_terminal.jpg)



and then eventually, just windows inside a graphical user interface.

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/putty1.jpg)

This is where we are now. Terminals still behave almost exactly the same way they did when they were invented in the 1960s.



Whenever you start a program on your computer it is attached to a terminal. By default, it is attached to the terminal in which you started it. Your operating system knows how to route inputs and outputs to your program, and not to some other program or device, because your program is attached to your terminal. In fact, there are likely hundreds of terminals currently attached in your operating system, attached to various devices and programs. Go ahead, have a look.

Type at the command prompt:


```shell
$ ls /dev/tty*
```

You can also find out the one to which your current shell is attached.

```shell
$ tty
```

A little more abstractly, you should think of a terminal as a thing with two ends. At one end, you attach a keyboard (input) and screen (output) and at the other end, you attach a program's input and output.


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/Terminal1.png)

---



A user types at a terminal (a physical teletype). This terminal is connected through a pair of wires to a **Universal Asynchronous Receiver and Transmitter** (**UART**) on the computer. The operating system contains a UART driver which manages the physical transmission of bytes, including parity checks and flow control.

In a naïve system, the UART driver would then deliver the incoming bytes directly to some application process.


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tty_case1.png)





But such an approach would lack the following essential features:

- **Line editing**.

    - Most users make mistakes while typing, so a backspace key is often useful. This could of course be implemented by the applications themselves, but in accordance with the UNIX design philosophy, applications should be kept as simple as possible. So as a convenience, the operating system provides an editing buffer and some rudimentary editing commands (backspace, erase word, clear line, reprint), which are enabled by default inside the **line discipline**. Advanced applications may disable these features by putting the line discipline in raw mode instead of the default cooked (or canonical) mode. Most interactive applications (editors, mail user agents, shells, all programs relying on curses or readline) run in raw mode, and handle all the line editing commands themselves. The line discipline also contains options for character echoing and automatic conversion between carriage returns and linefeeds. Think of it as a primitive kernel-level sed(1), if you like.

    Incidentally, the kernel provides several different line disciplines. Only one of them is attached to a given serial device at a time. The default discipline, which provides line editing, is called N_TTY (drivers/char/n_tty.c, if you're feeling adventurous). Other disciplines are used for other purposes, such as managing packet switched data (ppp, IrDA, serial mice), but that is outside the scope of this article.



- Session management.

    - The user probably wants to run several programs simultaneously, and interact with them one at a time. If a program goes into an endless loop, the user may want to kill it or suspend it. Programs that are started in the background should be able to execute until they try to write to the terminal, at which point they should be suspended.

    Likewise, user input should be directed to the foreground program only. The operating system implements these features in the **TTY driver** (drivers/char/tty_io.c).

    An operating system process is "alive" (has an execution context), which means that it can perform actions. The TTY driver is not alive; in object oriented terminology, the TTY driver is a passive object. It has some data fields and some methods, but the only way it can actually do something is when one of its methods gets called from the context of a process or a kernel interrupt handler. The line discipline is likewise a passive entity.

Together, a particular triplet of UART driver, line discipline instance and TTY driver may be referred to as a **TTY device**, or sometimes just **TTY**. A user process can affect the behavior of any TTY device by manipulating the corresponding device file under /dev.






the number in parentheses shown after Unix command names in manpages mean?
the section that the man page for the command is assigned to.

These are split as

1. General commands
2. System calls
3. C library functions
4. Special files (usually devices, those found in /dev) and drivers
5. File formats and conventions
6. Games and screensavers
7. Miscellanea
8. System administration commands and daemons








The physical line in the previous diagram could of course be a long-distance phone line:


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tty_case2.png)


This does not change much, except that the system now has to handle a modem hangup situation as well.

Let's move on to a typical desktop system. This is how the Linux console works:


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tty_case3.png)

The TTY driver and line discipline behave just like in the previous examples, but there is no UART or physical terminal involved anymore. Instead, a video terminal (a complex state machine including a frame buffer of characters and graphical character attributes) is emulated in software, and rendered to a VGA display.

The console subsystem is somewhat rigid. Things get more flexible (and abstract) if we move the terminal emulation into userland. This is how xterm(1) and its clones work:

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tty_case4.png)

To facilitate moving the terminal emulation into userland, while still keeping the TTY subsystem (session management and line discipline) intact, the pseudo terminal or pty was invented.

A pty is a pseudo-teletype port provided by a computer Operating System Kernel to connect user land terminal emulation software programs such as ssh, xterm, or screen.

And as you may have guessed, things get even more complicated when you start running pseudo terminals inside pseudo terminals, à la screen(1) or ssh(1).


Linux mounts a special file system dev/pts on /dev (the 's' presumably standing for serial) that creates a corresponding entry in /dev/pts for every new terminal window you open, e.g. /dev/pts/0






A pty is a pseudo-terminal - it’s a software implementation that appears to the attached program like a terminal, but instead of communicating directly with a “real” terminal, it transfers the input and output to another program. For example, when you ssh in to a machine and run ls, the ls command is sending its output to a pseudo-terminal, the other side of which is attached to the SSH daemon. A pts is the slave part of a pty. A ptmx is the master part of a pty.




Jobs and sessions
Job control is what happens when you press ^Z to suspend a program, or when you start a program in the background using &. A job is the same as a process group. Internal shell commands like jobs, fg and bg can be used to manipulate the existing jobs within a session. Each session is managed by a session leader, the shell, which is cooperating tightly with the kernel using a complex protocol of signals and system calls.

The following example illustrates the relationship between processes, jobs and sessions:

The following shell interactions...

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/exampleterm.png)

...correspond to these processes...


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/examplediagram.png)

...and these kernel structures.

> TTY Driver (/dev/pts/0).

> Size: 45x13

> Controlling process group: (101)

> Foreground process group: (103)

> UART configuration (ignored, since this is an xterm):
  Baud rate, parity, word length and much more.

>Line discipline configuration:
  cooked/raw mode, linefeed correction,
  meaning of interrupt characters etc.

> Line discipline state:
  edit buffer (currently empty),
  cursor position within buffer etc.


>pipe0

>Readable end (connected to PID 104 as file descriptor 0)

>Writable end (connected to PID 103 as file descriptor 1)
Buffer


- TTY has a very important attribute called foreground process group, which records the current front-end process group.

- When the tty driver, e.g., pts/0 receives the input, it will check which front-end process group is, and then put the input into the input cache of the leader of the process group, so that the corresponding leader process can get the user’s input through the read function

- When a process in the current process group writes data to the TTY device, TTY will output the data to the output device
When executing different commands in the shell, the front-end process group is constantly changing, and the shell is responsible for updating this change to the TTY device


在讨论TTY设备是如何被创建及配置之前，我们先来看看TTY是如何被进程使用的：

```shell
#  prints the file name of the terminal connected to standard input of the current bash
$ tty
/dev/pts/0

# list all processes that opened the file /dev/pts/0
$ lsof /dev/pts/0
COMMAND   PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
bash    72527 ubuntu    0u   CHR  136,0      0t0    3 /dev/pts/0
bash    72527 ubuntu    1u   CHR  136,0      0t0    3 /dev/pts/0
bash    72527 ubuntu    2u   CHR  136,0      0t0    3 /dev/pts/0
bash    72527 ubuntu  255u   CHR  136,0      0t0    3 /dev/pts/0
lsof    72549 ubuntu    0u   CHR  136,0      0t0    3 /dev/pts/0
lsof    72549 ubuntu    1u   CHR  136,0      0t0    3 /dev/pts/0
lsof    72549 ubuntu    2u   CHR  136,0      0t0    3 /dev/pts/0


```

From the above lsof, we can see that stdin (0U), stdout (1U) and stderr (2U) of the currently running Bash and lsof processes are bound to /dev/pts/0.


Let's see what happens when...


you type something in a terminal emulator in the user land like XTerm or any any other application you use to get a terminal. What it actually happens is:

- a GUI which emulates the terminal starts (like the Terminal or Xterm UI application).
- it draws the UI to the video and requests a pty from the OS.
- launches bash as subprocess

- The std input, output and error of the bash will be set to be the pty slave.

- XTerm listens for keyboard events and sends the characters to the pty master

- The line discipline gets the character and buffers them. It copies them to the slave only when you press enter. It also writes back its input to the master (echoing back). Remember the terminal is dumb, it will only show stuff on the screen if it comes from the pty master. Thus, the line discipline echoes back the character so that the terminal can draw it on the video, allowing you to see what you just typed.

- When you press enter, the TTY driver (it's 'just' a kernel module) takes care of copying the buffered data to the pty slave
- bash (which was waiting for input on standard input) finally reads the characters (for example 'ls -la'). Again, remember that bash standard input is set to be the PTY slave.

- At this points bash interprets the character and figures it needs to run 'ls'

- It forks the process and runs 'ls' in it.

- The forked process will have the same stdin, stdout and stderr used by bash, which is the PTY slave.
- ls runs and prints to standard output (once again, this is the pty slave)
the tty driver copies the characters to the master(no, the line discipline does not intervene on the way back)

- XTerm reads in a loop the bytes from the pty master and redraws the UI



## SSH远程访问


这里的Terminal可能是任何地方的程序，比如windows上的putty，所以不讨论客户端的Terminal程序是怎么和键盘、显示器交互的。由于Terminal要和ssh服务器打交道，所以肯定要实现ssh的客户端功能。

这里将建立连接和收发数据分两条线路解释，为了描述简洁，we use sshd whenever we need to refer to the ssh server program


Establishing a connection

1. Terminal request establishes connection with sshd

2. If the verification passes, sshd will create a new session


3. Call API (POSIX_ Openpt ()) requests ptmx to create a PTS. After successful creation, sshd will get the FD associated with ptmx and associate the FD with the session.

```shell
# pty (pseudo terminal device) is composed of two parts. ptmx is the master side and pts is the slave side
# the process can call the API to request ptmx to create a pts, and then it will get a read-write FD connected to ptmx and a newly created PTS

# ptmx maintains the corresponding relationship between the FD and PTS internally, and then the reading and writing to this FD will be forwarded to the corresponding PTS by ptmx

# you can see that sshd has opened /dev/ptmx

$ sudo lsof /dev/ptmx
COMMAND  PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    1143   root    5u   CHR    5,2      0t0   90 /dev/ptmx
sshd    1255 ubuntu   10u   CHR    5,2      0t0   90 /dev/ptmx
sshd    1255 ubuntu   12u   CHR    5,2      0t0   90 /dev/ptmx
sshd    1255 ubuntu   13u   CHR    5,2      0t0   90 /dev/ptmx

$ ls /dev/pt*
/dev/ptmx

/dev/pts:
0  ptmx

```




4.At the same time, sshd creates the shell process and binds the newly created pts to the shell.

So if we create a new ssh session to the same server:

```shell

$ ls /dev/pt*
/dev/ptmx

/dev/pts:
0  1  ptmx

$ sudo lsof /dev/ptmx
COMMAND  PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    1143   root    5u   CHR    5,2      0t0   90 /dev/ptmx
sshd    1255 ubuntu   10u   CHR    5,2      0t0   90 /dev/ptmx
sshd    1255 ubuntu   12u   CHR    5,2      0t0   90 /dev/ptmx
sshd    1255 ubuntu   13u   CHR    5,2      0t0   90 /dev/ptmx
sshd    1616   root    5u   CHR    5,2      0t0   90 /dev/ptmx
sshd    1698 ubuntu   10u   CHR    5,2      0t0   90 /dev/ptmx
sshd    1698 ubuntu   12u   CHR    5,2      0t0   90 /dev/ptmx
sshd    1698 ubuntu   13u   CHR    5,2      0t0   90 /dev/ptmx

```

in the new session window, typing tts, you will see

```shell
$ tty
/dev/pts/1

```

### Send and receive messages

1. Terminal receives the input from the keyboard and sends the data to sshd through SSH protocol

2. After receiving the data from the client, sshd finds the corresponding FD associated with ptmx according to the session it manages

3. Write the data sent by the client to the found FD

4. After receiving the data, ptmx finds the corresponding PTS according to FD (the corresponding relationship is automatically maintained by ptmx), and forwards the data packet to the corresponding PTS

5. After receiving the packet, PTS checks the current front-end process group bound to itself, and sends the packet to the leader of the process group

6. Since there is only shell on PTS, the read function of shell receives the packet

7. The shell processes the received packets and outputs the processing results (or no output)

8. Shell writes the result to PTS through write function

9. PTS forwards the results to ptmx

10. Ptmx finds the corresponding FD according to PTS and writes the result to the FD

11. After receiving the FD result, sshd finds the corresponding session and sends the result to the corresponding client

/dev/pts contains entries corresponding to devices. /dev/pts is a special directory that is created dynamically by the Linux kernel. The contents of the directory vary with time and reflect the state of the running system
