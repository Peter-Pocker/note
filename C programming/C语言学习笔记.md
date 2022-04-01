- [ ] 在一条printf语句中，若存在多个函数引用，函数表达式的执行顺序有时是从后往前，有时是从前往后

```c
i = 0;
printf("(i++, i--):i = %d, i = %d\n",i++, i--);
//(i++, i--):i = -1, i = 0
printf("(i++, --i):i = %d, i = %d\n",i++, --i);
//(i++, --i):i = -1, i = 0
printf("(++i, --i):i = %d, i = %d\n",++i, --i);
//(++i, --i):i = 0, i = 0
printf("(++i, --i):i = %d, i = %d\n",++i, i--);
//(++i, i--):i = 0, i = 0
```

原因：

函数参数的求值顺序依赖于编译器，例如f(a,a++);是先求a++还是求a不一定。C语言中的大多数运算符对其操作数的求值顺序也依赖于编译器。

##### C语言中的顺序点

指程序执行过程中修改变量值的最晚时刻。

###### 位置

1）分号；
2）未重载的逗号运算符的左操作数赋值之后（即'，'处）
3）未重载的'||'运算符的左操作数赋值之后（即'||'处）；
4）未重载的'&&'运算符的左操作数赋值之后（即“&&”处）；
5）三元运算符'？ ： '的左操作数赋值之后（即'？'处）；
6）在函数所有参数赋值之后但在函数第一条语句执行之前；
7）在函数返回值已拷贝给调用者之后但在该函数之外的代码执行之前；
8）每个基类和成员初始化之后；
9）在每一个完整的变量声明处有一个顺序点，例如int i， j；中逗号和分号处分别有一个顺序点；
10）for循环控制条件中的两个分号处各有一个顺序点。

宏定义

不加展开代码的宏定义，主要用于清除宏定义。因为编译器在预处理代码时，会替换宏定义为展开代码。

例如，在 Windows 64 位 的 mingw 的C:\mingw64\include\libiberty\ansidecl.h中，有如下代码

```c
/* some systems define these in header files for non-ansi mode */
#undef const
#undef volatile
#undef signed
#undef inline
#define const
#define volatile
#define signed
#define inline
/* 主要用于清除用非标准C编写的代码中的关键字 */
```

IEEE754 浮点数标准

![image-20220116164805985](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220116164805985.png)

注意指数以 2 为底

参考

[IEEE 754浮点数标准详解 (biancheng.net)](http://c.biancheng.net/view/314.html)

##### 位域

在C语言编程中，有时会看到变量声明后带冒号的写法，这其实是代表该变量控制字节中的几位

```c
/* 在 linux-5.11\include\linux\sched.h 中有这样一段定义 */
struct sched_dl_entity {
    /* 此处定义其他变量 */
    /* 依照声明顺序，这四个变量分别控制一个字节的前四位 */
    unsigned int			dl_throttled      : 1;
	unsigned int			dl_yielded        : 1;
	unsigned int			dl_non_contending : 1;
	unsigned int			dl_overrun	  : 1;
    /* 此处定义其他变量 */
}
```

Reference to [C语言变量声明加冒号的用法 - J博士 - 博客园 (cnblogs.com)](https://www.cnblogs.com/jacklu/p/4425963.html)

##### 

#### 类型转换

当表达式中存在有符号类型和无符号类型时所有的操作数都自动转换为无符号类型。

函数的第一个输入参数似乎具有默认值（全一），其余参数则不具有默认值。例如如下示例程序

```c
#include <stdio.h>
#include <stdlib.h>
// #include "new.c"
// static int i=2;
int fun3(void);
int fun4(int a, int b, int c);
int main(void) {
    int (*funp1)(void);
    funp1 = fun3;
    funp1();
    funp1 = (int (*)(void))fun4;
    // int n = 3;
    // int i = 1;
    // for (; n > 0; --n)
    funp1();
    return 0;
}

int fun3(void) {
    printf("fun\n");
    return 0;
}
int fun4(int a, int b, int c) {
    printf("fun4 : %d, %d, %d\n", a, b, c);
    return a;
}
// 输出 
fun
fun4 : -1, 3930672, 3955008
```

##### C语言位域

位域的声明与访问

```
type name:width
type 为类型名，可以选择 int, unsigned int, signed int
位域的访问与普通成员格式一样
structName.name
structNamePointer->name
```

> 一个字节不够存放另一个位域时，会从下一单元起存放位域，这是真的吗？为什么 16,2,7,7 的位域总共占用4个字节（都声明为int），mac上实验

#### 类型转换

C语言中类型转换遵循以下规则

1）整型之间相互转换时（long, int, short, char）

1. 长类型 --> 短类型

   截断高位 bit，保留低位 bit

2. 短类型 --> 长类型

   若短类型有符号，则高位 bit 由符号位填充，否则以0填充。填充数字与长类型是否为带符号型无关

3. 等长 bit 类型是否带符号转换

   保持二进制表示不变，改变解释方式

#### 函数调用开始为什么需要 sub $0x40,%rsp

用gdb反汇编一段简单的C语言代码编译产生的机器码时，可以看到在跳转到main函数之前，会先执行保存栈基址指针，修改栈顶指针的操作，还有一句 `sub $0x40,%rsp`。这是因为，系统会为程序预分配一个栈，栈的预分配的大小就是 64 字节。不过，并不是所有的程序的栈都是这么大，系统会更具程序中局部变量总大小进行一个合理分配，也就是说，会大于所有局部变量可能共存时的总大小。

### 参考

[IEEE 754浮点数标准详解 (biancheng.net)](http://c.biancheng.net/view/314.html)
