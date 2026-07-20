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