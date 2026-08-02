# 05-gdb调试器入门 - 视频内容文档

## 时间线概览
- 00:00–01:10  GDB简介与四大功能
- 01:10–03:24  调试能力培养
- 03:24–04:35  安装GDB
- 04:35–06:43  启动GDB的三种方式
- 06:43–07:20  编译带调试选项的程序
- 07:20–09:13  启动调试、运行程序、退出
- 09:13–11:29  查看源代码与清屏
- 11:29–13:40  断点、单步执行、继续运行
- 13:40–15:00  打印与修改变量
- 15:00–16:00  断点信息与删除
- 16:00–17:40  查看内存与寄存器

## 1. GDB简介
GDB是**GNU Project Debugger**的简称，属于GNU开源项目中的自由软件，遵循GPL发行许可证。它在Linux系统中被广泛使用，是一个用于调试C语言程序的强大工具。

GDB主要提供以下四个方面的帮助：

1. **启动程序并指定影响其行为的参数**：可以自定义程序启动条件，观察特定参数下的执行表现。
2. **设置断点使程序在指定条件处停止**：这是最常用的功能，可以在任意位置暂停程序，以便观察变量、内存和程序状态。
3. **分析程序停止时（如崩溃）的现场**：当程序发生crash或异常退出时，可以离线分析core文件，检查崩溃瞬间的堆栈、变量等信息。
4. **在程序运行时修改其状态**：可以通过GDB动态改变变量的值或程序的控制流，从而在不重新编译的情况下试验不同的修复方案。

![keyframe_001_160.4s](img/gdb_overview_160.4.jpg)

这些功能使得GDB成为开发人员在项目调试中不可或缺的工具。遇到问题时，建议先自己使用调试器进行分析，这有助于提升问题定位和解决的能力。

## 2. 安装GDB
在Ubuntu/Debian系统上，可以通过apt包管理器安装GDB：

```bash
sudo apt-get install gdb
```

安装完成后，可以通过 `gdb --version` 查看已安装的版本：

```bash
gdb --version
```

输出示例：

```
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04.2) 9.2
Copyright (C) 2020 Free Software Foundation, Inc.
...
```

![keyframe_002_255.3s](img/gdb_install_255.3.jpg)

## 3. 启动GDB的几种方式
GDB有三种主要的启动场景：

1. **直接调试可执行文件**：
   ```bash
   gdb <可执行文件>
   ```
   这是最常用的方式，直接加载二进制程序并在GDB环境中进行调试。

2. **调试正在运行的进程**：
   ```bash
   gdb -p <PID>
   ```
   适用于正在运行的服务或程序，前提是该进程包含调试信息且当前用户有足够权限。

   ![keyframe_003_282.6s](img/gdb_attach_282.6.jpg)

3. **调试core文件（程序崩溃转储）**：
   ```bash
   gdb <可执行文件> <core文件>
   ```
   当程序突然崩溃时，系统可能生成core dump文件，GDB可以加载该文件以分析崩溃时的内存和堆栈状态。

本视频重点讲解第一种方式。

## 4. 编译带调试信息的程序
为了让GDB能够读取符号表并进行源码级调试，在使用GCC编译时必须添加 `-g` 选项。例如，有一个简单的C程序 `1.c`：

```c
#include <stdio.h>

int main(void)
{
    int b;
    b = 20;
    printf("hello world \n");
    return 0;
}
```

编译命令：

```bash
gcc -o bin 1.c -g
```

这里 `-o bin` 指定输出文件名为 `bin`，`-g` 包含调试信息。

![keyframe_005_445.9s](img/gcc_compile_445.9.jpg)

## 5. GDB基本操作

### 5.1 启动与退出
启动GDB并加载程序：

```bash
gdb ./bin
```

默认会显示GNU版权信息和版本。如果希望界面更简洁，可以使用 `-q` 选项：

```bash
gdb ./bin -q
```

进入GDB交互界面后，命令提示符为 `(gdb)`。要退出GDB，输入 `q` 并回车。

![keyframe_006_449.0s](img/gdb_launch_449.0.jpg)

### 5.2 运行程序
在 `(gdb)` 提示符下输入 `run`（或简写 `r`）即可开始执行程序。程序将正常运行直到结束或遇到断点。

```gdb
(gdb) run
Starting program: /home/workspace/lesson/bin 
hello world
[Inferior 1 (process ...) exited normally]
```

如果没有任何断点，程序会完整执行并输出结果。

![keyframe_007_514.0s](img/hello_world_514.0.jpg)

### 5.3 查看源代码
使用 `list` 命令可以显示源代码。可以指定行号，GDB会以该行为中心显示上下若干行。

```gdb
(gdb) list 7
```

假设源代码有行号，则会显示第7行附近的代码。在Vim中可通过 `:set nu` 显示行号，以便与GDB中的行号对应。

![keyframe_008_552.8s](img/gdb_list_552.8.jpg)

![keyframe_009_558.9s](img/gdb_list_558.9.jpg)

### 5.4 清屏
在GDB内部，可以使用 `shell clear` 命令调用shell的清屏功能，或者直接按快捷键 `Ctrl+L` 清空终端界面，保持视图整洁。

### 5.5 设置断点
使用 `break` 命令（简写 `b`）在指定位置设置断点。可以指定行号或函数名。

- 按行号：`break 8`  在第8行设置断点。
- 按函数名：`break main` 在main函数入口设置断点。

设置断点后，再执行 `run`，程序会在断点处暂停，等待进一步调试。

```gdb
(gdb) break 8
Breakpoint 1 at 0x55555555515c: file 1.c, line 8.
(gdb) run
Starting program: /home/workspace/lesson/bin 
Breakpoint 1, main () at 1.c:8
8	    printf("hello world \n");
```

![keyframe_010_665.3s](img/breakpoint_665.3.jpg)

### 5.6 单步执行与继续运行
程序在断点处暂停后，可以使用单步调试命令逐行执行：

- `next`（简写 `n`）：执行下一行代码，不进入函数内部。
- `step`（简写 `s`）：单步进入函数内部。

要继续执行直到下一个断点或程序结束，使用 `continue`（简写 `c`）。

```gdb
(gdb) next
hello world
9	    return 0;
(gdb) c
Continuing.
[Inferior 1 (process 3820) exited normally]
```

![keyframe_011_668.8s](img/next_step_668.8.jpg)

### 5.7 打印变量值
当程序停在某一行时，可以使用 `print`（简写 `p`）命令查看变量的当前值。

```gdb
(gdb) print b
$1 = 20
```

![keyframe_012_759.6s](img/print_variable_759.6.jpg)

### 5.8 修改变量值
通过 `set var` 命令可以在调试时动态修改变量的值。例如将变量 `b` 改为30：

```gdb
(gdb) set var b = 30
(gdb) print b
$2 = 30
```

修改后程序继续运行时会使用新值，便于测试不同分支。

### 5.9 查看与删除断点
- 查看当前所有断点：`info breakpoints`（简写 `i b`）
  ```gdb
  (gdb) info breakpoints
  Num     Type           Disp Enb Address            What
  1       breakpoint     keep y   0x000055555555515c in main at 1.c:8
  ```
- 删除指定断点：`delete <断点编号>`（简写 `d <编号>`）
  ```gdb
  (gdb) delete 1
  ```
  再次查看时已无断点。

## 6. 查看内存和寄存器
GDB还可以直接访问底层数据，帮助分析内存布局和寄存器状态。

### 6.1 获取变量地址
使用 `print &变量名` 可以打印变量的内存地址：

```gdb
(gdb) print &b
$3 = (int *) 0x7fffffffe03c
```

### 6.2 使用 `x` 命令检查内存
`x` 命令（examine的缩写）用于查看内存区域的内容。常用格式为 `x /<格式> <地址>`。

- `x /d <地址>`：以十进制显示该地址的内容
- `x /x <地址>`：以十六进制显示

例如：

```gdb
(gdb) x /d 0x7fffffffe03c
0x7fffffffe03c: 20
(gdb) x /x 0x7fffffffe03c
0x7fffffffe03c: 0x00000014
```

![keyframe_013_940.4s](img/x_command_940.4.jpg)

### 6.3 查看寄存器
`info registers`（简写 `i r`）可以显示当前CPU各寄存器的值，有助于底层调试。

```gdb
(gdb) info registers
rax            0x555555555149   93824992235849
rbx            0x555555555170   93824992235888
rcx            0x555555555170   93824992235888
rdx            0x7fffffffe148   140737488347464
...
rip            0x55555555515c   0x55555555515c <main+19>
```

![keyframe_014_963.3s](img/registers_963.3.jpg)

---

以上即为GDB调试器入门的基础操作。实际开发中，熟练掌握这些命令可以大幅提升故障排查效率。后续应多动手实践，逐步掌握更高级的调试技巧。