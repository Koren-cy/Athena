## 编译器视角：理解他，并应用他

```c
#stm32f103xb.h
#define __O volatile /*!< Defines 'write only' permissions */
#define __IO volatile /*!< Defines 'read / write' permissions */

typedef struct
{
__IO uint32_t CRL;
__IO uint32_t CRH;
__IO uint32_t IDR;
__IO uint32_t ODR;
__IO uint32_t BSRR;
__IO uint32_t BRR;
__IO uint32_t LCKR;
} GPIO_TypeDef;
```

```c
#cmsis_gcc.h
#define __NO_RETURN
#define __USED
#define __WEAK
#define __PACKED
#define __PACKED_STRUCT
#define __PACKED_UNION
```

```c
#include/linux/init.h
#ifndef MODULE
// 省略
#define module_init(x) __initcall(x);
// 省略
#else
#define module_init(x) __initcall(x);
|
--> #define __initcall(fn) device_initcall(fn)
    |
    --> #define device_initcall(fn) __define_initcall(fn, 6)
    |
    --> #define __define_initcall(fn, id) \
        static initcall_t __initcall_##fn##id __used \
        __attribute__((__section��"."initcall" #id ".init"))) = fn
```