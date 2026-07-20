## keyframe_001_160.4s (160.4s)

## 2.4 程序调试器gdb
## 2.4.1 gdb简介
gdb是GNU Project Debugger的简称，也是GNU(GNU's Not Unix)开源项目中遵循GPL发行许可证的free software。掌握gdb调试技巧，在项目上遇到问题才会得心应手。gdb 官方网站(https://sourceware.org/gdb/)
一般来说，GDB主要能够提供以下四个方面的帮助：
GDB can do four main kinds of things (plus other things in support of these) to help you catch bugs in the act:
Start your program, specifying anything that might affect its behavior.(指定—些参数)
• Make your program stop on specified conditions. (断点)
• Examine what has happened, when your program has stopped.(分析crash现场)
• Change things in your program, so you can experiment with correcting the effects of one bug and go on to learn about another.(直接修改程序，看结果)
## GDB: The GNU Project Debugger [bugs] [maintainers] [contributing] [current git] [documentation] [download] [home] [irc] [links] [mailing lists] [news] [schedule] [song] [wiki] GDB: The GNU Project Debugger
## What is GDB?
GDB, the GNU Project debugger, allows you to see what is going on inside' another program while it executes -- or what another program was doing at the moment it crashed.
GDB can do four main kinds of things (plus other things in support of these) to help you catch bugs in the act:
• Start your program, specifying anything that might affect its behavior.
• Make your program stop on specified conditions.
• Examine what has happened, when your program has stopped.
• Change things in your program, so you can experiment with correcting the effects of one bug and go on to learn about another.
Those programs might be executing on the same machine as GDB (native), on another machine (remote), or on a simulator. GDB can run on most popular UNIX and Microsoft Windows variants, as well as on macOS

---

## keyframe_002_255.3s (255.3s)

murphy@ubuntu:/home/workspace/lesson\$ sudo apt-get install gbd
[sudo] password for murphy:
murphy@ubuntu:/home/workspace/lesson\$ sudo apt-get install gdb
[sudo] password for murphy:
Reading package lists... Done
Building dependency tree
Reading state information... Done
gdb is already the newest version (9.2-0ubuntul\~20.04.2).
0 upgraded, 0 newly installed, 0 to remove and 99 not upgraded.
murphy@ubuntu:/home/workspace/lesson\$ gdb --version
GNU gdb (Ubuntu 9.2-0ubuntu1\~20.04.2) 9.2
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html> This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
murphy@ubuntu:/home/workspace/lesson\$ g■

---

## keyframe_003_282.6s (282.6s)

<table><tr><td>1953 ? 1955 ? 1958 ? 1964 ? 1966 ? 1974 1977 1981 ? 1991 ? 1998 ? 1999 ? 2006 ? 2008 2011 ? 2017 ? 2030 ? 2072 ? 2137 ? 2142 ? 3037 ? 3171 ? 3227 ? 3237 pts/0 3461 ? 3467 3618 ? 3642 ? 3650 ? 3727 ? 3741 pts/0</td><td>00:00:00 gvfsd-trash 00:00:00 gsd-power 00:00:00 gsd-print-notif 00:00:00 gsd-rfkill 00:00:00 gsd-disk-utilit 00:00:00 gsd-screensaver 00:00:00 gsd-sharing 00:00:00 gsd-smartcard 00:00:00 gsd-sound 00:00:00 gsd-usb-protect 00:00:00 gsd-wacom 00:00:00 gsd-wwan 00:00:00 gsd-xsettings 00:01:09 vmtoolsd 00:00:00 ibus-engine-sim 00:00:00 evolution-alarm 00:00:00 gsd-printer 00:00:00 gvfsd-metadata 00:00:01 update-notifier 00:00:00 kworker/u256:2-events unbound 00:00:00 kworker/0:0-cgroup_destroy 00:00:11 gnome-terminal- 00:00:00 bash 00:00:00 kworker/u256:1-events unbound 00:00:00 kworker/0:1-events 00:00:01 kworker/1:3-events 00:00:00 kworker/1:0-cgroup_destroy 00:00:00 kworker/u256:0-events_power_efficient 00:00:00 kworker/0:2-events 00:00:00 ps murphy@ubuntu:/home/workspace/lesson$ gdb -p 3741■</td></tr></table>

---

## keyframe_004_427.3s (427.3s)

(no text detected)

---

## keyframe_005_445.9s (445.9s)

murphy@ubuntu:/home/workspace/lesson\$ vim 1.c
murphy@ubuntu:/home/workspace/lesson\$ gcc -o bin 1.c -g murphy@ubuntu:/home/workspace/lesson\$ gdb ./bin
GNU gdb (Ubuntu 9.2-0ubuntu1\~20.04.2) 9.2
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/ licenses/gpl.html>
This is free software: you are free to change and redistrib ute it.
There is NO WARRANTY, to the extent permitted by law. Type "show copying" and "show warranty" for details. This GDB was configured as "x86 64-linux-gnu" Type "show configuration" for configuration details. For bug reporting instructions, please see: <http://www.gnu.org/software/gdb/bugs/>. Find the GDB manual and other documentation resources onlin e at: <http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word Reading symbols from ./bin...
(gdb) q
murphy@ubuntu:/home/workspace/lesson\$
## #include <stdio.h>
int main(void) int b; b = 20; printf("hello world \n"); return 0;

---

## keyframe_006_449.0s (449.0s)

License GPLv3+: GNU GPL version 3 or later <http://gnu.org/#include <stdio.h> licenses/gpl.html>
This is free software: you are free to change and redistrib int main(void) ute it.
There is NO WARRANTY, to the extent permitted by law. Type "show copying" and "show warranty" for details. This GDB was configured as "x86 64-linux-gnu". Type "show configuration" for configuration details. For bug reporting instructions, please see: <http://www.gnu.org/software/gdb/bugs/>. Find the GDB manual and other documentation resources onlin e at: <http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word Reading symbols from ./bin...
(gdb) q
murphy@ubuntu:/home/workspace/lesson\$ cler
Command 'cler' not found, did you mean:
command 'cleo' from deb cleo (0.004-2) command 'clear' from deb ncurses-bin (6.2-0ubuntu2.1) command 'clex' from deb clex (4.6.patch8-1) command 'cver' from deb gplcver (2.12a-1.1build1)
Try: sudo apt install <deb name>
int b;
b = 20;
printf("hello world \n");
return 0;

---

## keyframe_007_514.0s (514.0s)

murphy@ubuntu:/home/workspace/lesson\$ gdb ./bin -q Reading symbols from ./bin..
Starting program: /home/workspace/lesson/bin hello world
$$
\boxed { \bullet } \boxed { \bullet } \boxed { \bullet }
$$
#include <stdio.h>

---

## keyframe_008_552.8s (552.8s)

murphy@ubuntu:/home/workspace/lesson\$ gdb ./bin -q Reading symbols from ./bin..
3 int main(void)
456789 {
int b;
b = 20;
printf("hello world \n");
return 0;
10 } T
(gdb) ■
```c
1 #include <stdio.h>
2
3 int main(void)
4 {
5 ☑int b;
6
7 b = 20;
8 printf("hello world \n");
9 return 0;
10 }
```

---

## keyframe_009_558.9s (558.9s)

```c
3 int main(void)
456789 {
int b;
b = 20;
printf("hello world \n");
return 0;
10 }
(gdb)
```
murphy@ubuntu:/home/workspace/lesson\$ gdb ./bin -q Reading symbols from ./bin...
```c
1 #include <stdio.h>
2
3 int main(void)
4 {
56 ☑int b;
7 b = 20;
8 printf("hello world \n");
9 return 0;
10 }
```

---

## keyframe_010_665.3s (665.3s)

(gdb) break 8
Breakpoint 1 at 0x55555555515c: file 1.c, line 8. (gdb) run
Starting program: /home/workspace/lesson/bin
Breakpoint 1, main () at 1.c:8 printf("hello world \n");
[Inferior 1 (process 3820) exited normally] (gdb)

---

## keyframe_011_668.8s (668.8s)

(gdb) break 8
Breakpoint 1 at 0x55555555515c: file 1.c, line 8.
(gdb) run
Starting program: /home/workspace/lesson/bin
Breakpoint 1, main () at 1.c:8
8 printf("hello world \n");
(gdb) next
hello world
9 return 0;
(gdb) c
Continuing.
[Inferior 1 (process 3820) exited normally]
(gdb)
I
```c
1 #include <stdio.h>
234567 int main(void)
{
int b;
b = 20;
89 printf("hello world\n");
return 0;
10 }
```

---

## keyframe_012_759.6s (759.6s)

## (gdb) run Starting program: /home/workspace/lesson/bin
Breakpoint 1, main () at 1.c:8 printf("hello world \n");
1 #include <stdio.h>
int main(void)
int b;
b = 20;
printf("hello world \n");
return 0;

---

## keyframe_013_940.4s (940.4s)

(gdb) break 8
Breakpoint 2 at 0x55555555515c: file 1.c, line 8. (gdb) run
Starting program: /home/workspace/lesson/bin
Breakpoint 2, main () at 1.c:8 printf("hello world \n");
(gdb) x /d 0x7fffffffe03c
0x7fffffffe03c: 20
(gdb) info re
int b;

---

## keyframe_014_963.3s (963.3s)

<table><tr><td>]+[ $4 = 20</td><td>murphy@ubuntu:/home/workspace/lesson</td></tr><tr><td>(gdb) x 0x7fffffffe03c</td><td></td></tr><tr><td>0x7fffffffe03c: 0x00000014 (gdb) x /d 0x7fffffffe03c</td><td></td></tr><tr><td>0x7fffffffe03c: 20</td><td></td></tr><tr><td colspan="2">(gdb) info register</td></tr><tr><td>rax 0x555555555149</td><td>93824992235849</td></tr><tr><td>rbx</td><td>0x555555555170 93824992235888</td></tr><tr><td>rcx</td><td>0x555555555170 93824992235888</td></tr><tr><td>rdx</td><td>0x7fffffffe148 140737488347464</td></tr><tr><td>rsi 0x7fffffffe138</td><td>140737488347448</td></tr><tr><td>rdi 0x1</td><td>1 0x7fffffffe040</td></tr><tr><td>rbp</td><td>0x7fffffffe040</td></tr><tr><td>rsp</td><td>0x7fffffffe030 0x7fffffffe030</td></tr><tr><td>r8 0x0</td><td>0</td></tr><tr><td>r9 0x7ffff7fe0d60</td><td>140737354009952</td></tr><tr><td>r10 0x0</td><td>0 0x7ffff7f728f0 140737353558256</td></tr><tr><td>r11</td><td></td></tr><tr><td>r12</td><td>0x555555555060 93824992235616</td></tr><tr><td>r13 0x7fffffffe130</td><td>140737488347440</td></tr><tr><td>r14 0x0</td><td>0</td></tr><tr><td>r15 0x0</td><td>0</td></tr><tr><td>rip</td><td>0x55555555515c 0x55555555515c &lt;main+19&gt;</td></tr><tr><td>eflags 0x206</td><td>[ PF IF ]</td></tr><tr><td>CS 0x33</td><td>51</td></tr><tr><td>SS 0x2b</td><td>43</td></tr><tr><td>ds 0x0</td><td>0</td></tr><tr><td>0x0</td><td></td></tr><tr><td>es</td><td>0</td></tr><tr><td>fs 0x0</td><td>0</td></tr><tr><td>gs 0x0</td><td>0</td></tr></table>

---
