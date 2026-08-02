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