C++中的自动for循环

如果用自动循环遍历多维数组中的所有元素，除了最内层循环的循环变量可以不声明为引用，其他外层循环必须声明为引用

![image-20220305123757185](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220305123757185.png)

关于左值右值

![image-20220305125638900](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220305125638900.png)

运算符左右结合问题

![image-20220306100855624](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220306100855624.png)

在逻辑运算符中，只有`!`是右结合，其余均是左结合

C++中，对于有符号位的整型的移位操作是没有固定规定的！！！

![image-20220306105153210](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220306105153210.png)

移位运算符（又叫IO运算符)满足左结合

关于运算过程中的有符号类型和无符号类型的转换

原则

1. 低长度的变量向高长度的变量转
2. 同样长度的变量，有符号向无符号转

例子：

```
unsigned int op int == unsigned int op unsigned int
unsigned int op long long == long long op long long
```

浮点型和整型运算时，依据整型是否有符号决定整型的值

```
2.1+(unsigned int)(-1)=4.29497e+09
```

##### 强制类型转换

###### static_cast

可以将类型强制转换为更“短”的类型，并且不会产生警告。但是不能将const变量强制转换为非const变量，这样会引发报错。

###### const_cast

可以将指向const变量的指针或引用转换为非const指针

###### reinterpret_cast

可以进行不同类型指针之间的转换（非const）

###### dynamic_cast

C++ 标![image-20220306231026231](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220306231026231.png)准错误库stdexcept定义的错误类型



## Q&A

