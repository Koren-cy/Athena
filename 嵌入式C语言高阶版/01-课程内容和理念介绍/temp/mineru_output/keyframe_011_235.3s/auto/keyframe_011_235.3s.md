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