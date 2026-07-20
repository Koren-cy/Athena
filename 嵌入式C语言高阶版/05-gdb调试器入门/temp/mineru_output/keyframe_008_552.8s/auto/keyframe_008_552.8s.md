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