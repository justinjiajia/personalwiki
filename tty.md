


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




A user types at a terminal (a physical teletype). This terminal is connected through a pair of wires to a UART (Universal Asynchronous Receiver and Transmitter) on the computer. The operating system contains a UART driver which manages the physical transmission of bytes, including parity checks and flow control. In a naïve system, the UART driver would then deliver the incoming bytes directly to some application process.


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tty_case1.png)



But such an approach would lack the following essential features:

- **Line editing**.

    - Most users make mistakes while typing, so a backspace key is often useful. This could of course be implemented by the applications themselves, but in accordance with the UNIX design philosophy, applications should be kept as simple as possible. So as a convenience, the operating system provides an editing buffer and some rudimentary editing commands (backspace, erase word, clear line, reprint), which are enabled by default inside the **line discipline**. Advanced applications may disable these features by putting the line discipline in raw mode instead of the default cooked (or canonical) mode. Most interactive applications (editors, mail user agents, shells, all programs relying on curses or readline) run in raw mode, and handle all the line editing commands themselves. The line discipline also contains options for character echoing and automatic conversion between carriage returns and linefeeds. Think of it as a primitive kernel-level sed(1), if you like.

    Incidentally, the kernel provides several different line disciplines. Only one of them is attached to a given serial device at a time. The default discipline, which provides line editing, is called N_TTY (drivers/char/n_tty.c, if you're feeling adventurous). Other disciplines are used for other purposes, such as managing packet switched data (ppp, IrDA, serial mice), but that is outside the scope of this article.



- Session management.

    - The user probably wants to run several programs simultaneously, and interact with them one at a time. If a program goes into an endless loop, the user may want to kill it or suspend it. Programs that are started in the background should be able to execute until they try to write to the terminal, at which point they should be suspended. Likewise, user input should be directed to the foreground program only. The operating system implements these features in the TTY driver (drivers/char/tty_io.c).

    An operating system process is "alive" (has an execution context), which means that it can perform actions. The TTY driver is not alive; in object oriented terminology, the TTY driver is a passive object. It has some data fields and some methods, but the only way it can actually do something is when one of its methods gets called from the context of a process or a kernel interrupt handler. The line discipline is likewise a passive entity.

Together, a particular triplet of UART driver, line discipline instance and TTY driver may be referred to as a **TTY device**, or sometimes just TTY. A user process can affect the behaviour of any TTY device by manipulating the corresponding device file under /dev.






The physical line in the previous diagram could of course be a long-distance phone line:

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tty_case2.png)


This does not change much, except that the system now has to handle a modem hangup situation as well.

Let's move on to a typical desktop system. This is how the Linux console works:

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tty_case3.png)

The TTY driver and line discipline behave just like in the previous examples, but there is no UART or physical terminal involved anymore. Instead, a video terminal (a complex state machine including a frame buffer of characters and graphical character attributes) is emulated in software, and rendered to a VGA display.

The console subsystem is somewhat rigid. Things get more flexible (and abstract) if we move the terminal emulation into userland. This is how xterm(1) and its clones work:

![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/tty_case4.png)

To facilitate moving the terminal emulation into userland, while still keeping the TTY subsystem (session management and line discipline) intact, the pseudo terminal or pty was invented. And as you may have guessed, things get even more complicated when you start running pseudo terminals inside pseudo terminals, à la screen(1) or ssh(1).
