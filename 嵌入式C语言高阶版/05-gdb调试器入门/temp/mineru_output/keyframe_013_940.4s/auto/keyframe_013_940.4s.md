(gdb) break 8   
Breakpoint 2 at 0x55555555515c: file 1.c, line 8. (gdb) run   
Starting program: /home/workspace/lesson/bin

Breakpoint 2, main () at 1.c:8 printf("hello world \n");

(gdb) x /d 0x7fffffffe03c

0x7fffffffe03c: 20

(gdb) info re

int b;