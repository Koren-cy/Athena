## keyframe_001_13.5s (13.5s)

## 嵌入式C语言(高阶版)
To be a programmer, not just a coder
Murphy

---

## keyframe_002_32.4s (32.4s)

## C语言在操作系统中的地位

---

## keyframe_003_78.1s (78.1s)

## C语言是最靠近硬件的高级语言
计算机0层架构
Android 架构
编程语言

---

## keyframe_004_87.9s (87.9s)

手机
无人机
IOT设备
扫地机器人
智能驾驶舱

---

## keyframe_005_93.2s (93.2s)

C语言是嵌入式开发必备的工具和技能

---

## keyframe_006_98.7s (98.7s)

linux
RTOS
AutoSar
单片机裸机

---

## keyframe_007_107.3s (107.3s)

## 从三大视角，剖析C语言的底层逻辑

---

## keyframe_008_146.3s (146.3s)

## 内存视角：C语言控制硬件的本质是操作内存

---

## keyframe_009_161.0s (161.0s)

## 内存视角：CPU眼里，外设资源就是内存映射

---

## keyframe_010_195.9s (195.9s)

## 架构视角：C语言也可以很“面向对象”

---

## keyframe_011_235.3s (235.3s)

## 编译器视角：理解他，并应用他
1 #cmsis\_gcc.h
1 #stm32f103xb.h
2 #define \_0 volatile
3 #define \_\_IO volatile
5 typedef struct
6 {
7 \_\_IO uint32\_t CRL;
/\*!< Defines 'read / write' permissions \*/
/\*!< Defines 'write only' permissions \*/
8 \_\_IO uint32\_t CRH;
9 \_\_IO uint32\_t IDR;
11 \_\_IO uint32\_t BSRR;
10 \_\_IO uint32\_t ODR;
2 #define \_\_NO\_RETURN
12 \_\_IO uint32\_t BRR;
13 \_\_IO uint32\_t LCKR;
\_\_attribute\_\_((\_\_noreturn\_\_))
14 }GPIO\_TypeDef;
3 #define \_\_USED
\_\_attribute\_\_((used))
4 #define \_\_WEAK
\_\_attribute\_\_((weak))
5 #define \_\_PACKED
\_\_attribute\_\_((packed, aligned(1)))
union \_\_attribute\_\_((packed, aligned(1)))
6 #define \_\_PACKED\_STRUCT
7 #define \_\_PACKED\_UNION
struct \_\_attribute\_\_((packed, aligned(1)))
1 #include/linux/init.h
2 #ifndef MODULE
3 // 省略
4 #define module\_init(x) \_\_initcall(x);
5 // 省略
6 #else
8 #define module\_init(x) \_\_initcall(x);
9 1
10 --> #define \_\_initcall(fn) device\_initcall(fn)
111
12 --> #define device\_initcall(fn) \_\_define\_initcall(fn, 6)
13
14 --> #define \_\_define\_initcall(fn, id) \
15 static initcall\_t \_\_initcall\_##fn##id \_\_used \
16 \_\_attribute\_\_((\_\_section\_\_(".initcall" #id ".init"))) = fn

---

## keyframe_012_262.1s (262.1s)

## 深层次解构C语言底层逻辑：
三大视角：编译器、函数(架构设计)、内存
## 体会C语言的设计哲学：
站在C语言设计者的角度来思考，为什么要这样设计?
## 沉浸式Linux环境编程体验：
学习行业内优秀案例，拓宽编程视野，提升工作软实力

---
