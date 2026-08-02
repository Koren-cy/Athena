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