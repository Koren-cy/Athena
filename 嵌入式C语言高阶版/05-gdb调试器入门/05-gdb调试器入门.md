# GDB调试器入门

## GDB简介

GDB是GNU Debugger（GNU项目调试器）的简称，属于GNU开源项目中的自由软件，遵循GPL许可证。在Linux系统中被广泛用作C语言程序的调试工具。

GDB主要提供以下四个功能：

- **启动程序并指定影响其行为的参数**：可自定义启动条件，观察不同参数下的程序行为。
- **设置断点使程序在指定条件处停止**：在任意位置暂停程序，观察变量、内存和程序状态。
- **分析程序停止时（如崩溃）的现场**：当程序发生crash或异常退出时，可分析core文件，检查堆栈、变量等信息。
- **在程序运行时修改其状态**：可动态改变变量值或控制流，试验不同的修复方案，无需重新编译。

![keyframe_001_160.4s](img/gdb_overview_160.4.jpg)

*图：GDB功能概览*

使用GDB进行分析有助于提升问题定位和解决能力。

---

## 安装GDB

在Ubuntu/Debian系统上，通过apt包管理器安装。

```bash
sudo apt-get install gdb
```

安装完成后，查看版本以验证安装：

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

*图：GDB版本信息*

---

## 启动GDB的几种方式

GDB有三种主要启动场景：

1. **直接调试可执行文件**：`gdb <可执行文件>`，最常用方式。
2. **调试正在运行的进程**：`gdb -p <PID>`，适用于服务型程序。需确保进程包含调试信息且权限足够。

   ![keyframe_003_282.6s](img/gdb_attach_282.6.jpg)

   *图：附加到进程调试*
3. **调试core文件（程序崩溃转储）**：`gdb <可执行文件> <core文件>`，分析崩溃时的内存和堆栈状态。

本部分重点讲解第一种方式。

---

## 编译带调试信息的程序

为使GDB读取符号表并支持源码级调试，使用GCC编译时须添加 `-g` 选项。

示例程序 `1.c`：

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

**代码解析**
- `int b;`：声明整型变量b。
- `b = 20;`：为变量b赋值。
- `printf("hello world \n");`：输出字符串。

编译命令：

```bash
gcc -o bin 1.c -g
```

**命令解析**
- `gcc`：调用GNU编译器。
- `-o bin`：指定输出文件名为bin。
- `1.c`：源文件。
- `-g`：包含调试信息。

![keyframe_005_445.9s](img/gcc_compile_445.9.jpg)

*图：GCC编译命令*

---

## GDB基本操作

### 启动与退出

启动GDB并加载程序：

```bash
gdb ./bin
```

默认显示版权信息。使用 `-q` 选项启动简洁界面：

```bash
gdb ./bin -q
```

进入GDB交互界面后，命令提示符为 `(gdb)`。退出GDB时输入 `q` 并按回车。

![keyframe_006_449.0s](img/gdb_launch_449.0.jpg)

*图：GDB启动界面*

### 运行程序

在 `(gdb)` 提示符下输入 `run`（或简写 `r`）开始执行程序。若无断点，程序正常完成并输出结果：

```gdb
(gdb) run
Starting program: /home/workspace/lesson/bin 
hello world
[Inferior 1 (process ...) exited normally]
```

![keyframe_007_514.0s](img/hello_world_514.0.jpg)

*图：程序正常运行*

### 查看源代码

使用 `list` 命令显示源代码。可指定行号，GDB以该行为中心显示上下文：

```gdb
(gdb) list 7
```

在文本编辑器（如Vim）中可通过 `:set nu` 显示行号，以便对应GDB中的行号。

![keyframe_008_552.8s](img/gdb_list_552.8.jpg)

*图：list命令显示代码*

![keyframe_009_558.9s](img/gdb_list_558.9.jpg)

*图：Vim中显示行号*

### 清屏

在GDB内，使用 `shell clear` 命令调用shell清屏，或按快捷键 `Ctrl+L` 清空终端界面。

### 设置断点

使用 `break` 命令（简写 `b`）在指定位置设置断点。可指定行号或函数名：

- 按行号：`break 8`，在第8行设置断点。
- 按函数名：`break main`，在main函数入口设置断点。

设置断点后执行 `run`，程序在断点处暂停：

```gdb
(gdb) break 8
Breakpoint 1 at 0x55555555515c: file 1.c, line 8.
(gdb) run
Starting program: /home/workspace/lesson/bin 
Breakpoint 1, main () at 1.c:8
8	    printf("hello world \n");
```

![keyframe_010_665.3s](img/breakpoint_665.3.jpg)

*图：断点设置与触发*

### 单步执行与继续运行

程序在断点暂停后，可用单步调试命令逐行执行：

- `next`（简写 `n`）：执行下一行代码，不进入函数内部。
- `step`（简写 `s`）：单步进入函数内部。

继续执行至下一断点或程序结束，使用 `continue`（简写 `c`）。

```gdb
(gdb) next
hello world
9	    return 0;
(gdb) c
Continuing.
[Inferior 1 (process 3820) exited normally]
```

![keyframe_011_668.8s](img/next_step_668.8.jpg)

*图：单步执行过程*

### 打印变量值

程序停止时，可用 `print`（简写 `p`）查看变量当前值。

```gdb
(gdb) print b
$1 = 20
```

![keyframe_012_759.6s](img/print_variable_759.6.jpg)

*图：打印变量*

### 修改变量值

使用 `set var` 命令动态修改变量值。例如将变量 `b` 改为30：

```gdb
(gdb) set var b = 30
(gdb) print b
$2 = 30
```

修改后程序继续运行使用新值，便于测试不同分支。

### 查看与删除断点

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

---

## 查看内存和寄存器

GDB可直接访问底层数据，分析内存布局和寄存器状态。

### 获取变量地址

使用 `print &变量名` 打印变量的内存地址：

```gdb
(gdb) print &b
$3 = (int *) 0x7fffffffe03c
```

### 使用 `x` 命令检查内存

`x` 命令（examine的缩写）用于查看内存区域内容。常用格式为 `x /<格式> <地址>`。

- `x /d <地址>`：以十进制显示内容
- `x /x <地址>`：以十六进制显示

示例：

```gdb
(gdb) x /d 0x7fffffffe03c
0x7fffffffe03c: 20
(gdb) x /x 0x7fffffffe03c
0x7fffffffe03c: 0x00000014
```

![keyframe_013_940.4s](img/x_command_940.4.jpg)

*图：x命令输出*

### 查看寄存器

`info registers`（简写 `i r`）显示当前CPU各寄存器的值，适用于底层调试。

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

*图：寄存器状态*

---

## 小结

- GDB是Linux下C语言程序调试的核心工具，支持断点设置、变量查看与修改。
- 编译时需添加 `-g` 选项以包含调试信息，支持源码级调试。
- 基本操作包括启动程序、设置断点、单步执行和变量打印，满足大部分日常调试需求。
- 高级功能涉及内存检查和寄存器查看，辅助底层故障分析。