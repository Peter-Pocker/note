KEYWORDS

类继承	编程技巧

Java传递参数

向函数传入数组参数或者类的实例可以改变数组元素值或类实例（对象）的成员变量，不能改变字符串值，也不能直接改变int型的值

Error: No enclosing instance of type test is accessible. Must qualify the allocation with an enclosing instance of type xx

错误原因

在静态方法中调用了动态方法。可能出现在：在类的静态方法中实例化类中定义的私有非静态类，即new了一个对象。

解决办法

将动态方法修饰为静态方法。或者将对应私有类修饰为静态类

java中的判断等于

==

若执行 a = b 则 a == b 为true

```java
        String a = new String("abc");
        String b = a;
        String c = new String("abc");
        System.out.println(a==c);
        // false
        System.out.println(a==b);
        // true
```

不能用类型参数实例化数组，这是因为不能保证泛型一定有无参数的构造函数。在用Object做类型转换的时候，可能会产生警告。如果能够确保可以安全转换，可以加上注解过滤警告

```java
@SuppressWarnings("unchecked")
public class MyArray<E>{
	// code
		array = new E[10]; // 将报错
		array = (E[]) new Object[10]; // 正确的做法
}
```

在ArrayList数据结构的定义中：

```Java
    int indexOfRange(Object o, int start, int end) // 在左闭右开区间[start,end)中寻找o，而非闭区间
```

函数并不能改变对象型输入参数的指向

```java
public static void fun(int[] a){
    a = new int[]{1,1};
}
a = new int[]{0,1,2};
fun(a);
// 此时a仍然为{0,1,2},而并没有指向{1,1}
```

### 问题

在ArrayList.java中

```Java
    public void add(int index, E element) {
        rangeCheckForAdd(index);
        modCount++;
        final int s;
        Object[] elementData;
        if ((s = size) == (elementData = this.elementData).length) 
            // 这里为什么要把类成员变量赋值给一个新变量来进行比较呢？直接比较不行吗？
            elementData = grow();
        System.arraycopy(elementData, index,
                         elementData, index + 1,
                         s - index);
        elementData[index] = element;
        size = s + 1;
    }
```

回答：在Java内库源代码中处处可见这样的写法。往往是为了避免对变量造成修改

当求两个整型的平均值，而二者相加可能发生溢出时，可以这样写

```java
int mid = low + (high - low) / 2;
```

当程序中出现大量字符串拼接（"+"）的操作时，使用StringBuilder类的append方法比“+”更快，占用内存更少

```java
StringBuilder str = new StringBuilder();
str.append("somstring");
// ...
String res = str.toString();
```

【编程技巧】很牛的技巧！利用位运算交换两个基本类型变量的值

```java
a = a ^ b;
b = b ^ a;
a = a ^ b;
```

【编程技巧】利用HashSet可以很快地检查一个对象是否在一个集合或序列中，但是并不能减小空间复杂度

```java
HashSet<String> set=new HashSet<String>();
// 用HashSet的add方法向set添加元素，于是就有两种方法判断一个String的hash值在不在其中
// 方法1：根据add()的返回值，返回false则表示set中已有该String
// 方法2：contains()方法
```

【编程技巧】利用位运算判断奇偶

```java
(n & 1) == 1;
// true 奇数
// false 偶数
```

【编程技巧】求两个整数的平均数时，防止溢出的写法

```java
l + (r - l) / 2;
```

【接口】接口可以作为引用类型使用。任何实现该接口的类的实例都可以存储在接口类型的变量中，通过这些实例可以访问接口中的方法。程序运行时，Java动态地决定使用哪个类中的方法

【修饰符】`native` 修饰符

表明方法是由C/C++编写，具体实现在.ddl动态链接文件中

【基础语法，泛型编程】Java中使用泛型时，实例化的时候，泛型不能指定为基本类型，否则会报错，而应该指定为引用类型（包括数组类型）

```java
LinkedList<> list = new LinkedList(); // 正常
LinkedList<Integer> list = new LinkedList<Integer>(); // 正常
LinkedList<int[]> list = new LinkedList<int[]>(); // 正常
LinkedList<int> list = new LinkedList<int>(); // 报错
LinkedList<long> list = new LinkedList<long>(); // 即使是64位的长整形也会报错
```

【char类型】关于Java中，char到底是几个字节的解释

计算机编码分类

1. **内码** :某种语言运行时，其char和string在内存中的编码方式。
2. **外码** :除了内码，皆是外码。

对于Java而言，JVM的内码是UTF-16，所以Java的char在内存中是两个字节，不过有些字符需要两个char来存储

【文件操作】java.io中 `mkdir`方法与 `mkdirs`方法的区别

mkdir判断目录的父目录是否存在，若不存在则创建失败，而mkdirs则会创建本不存在的父目录

【基础语法】`break 代码标记`直接跳出到代码标记指向的位置，同理还有`continue label`，而Java中是没有`goto`关键字的

【基础语法，默认可见类型】使用默认访问修饰符声明的变量和方法，对同一个包内的类是可见的。接口里的变量都隐式声明为 **public static final**,而接口里的方法默认情况下访问权限为 **public**

【基础语法】Java中的数值类型不存在无符号的，它们的取值范围是固定的，不会随着机器硬件环境或者操作系统的改变而改变。

实际上，Java中还存在另外一种基本类型`void`，它也有对应的包装类`java.lang.Void`，不过我们无法直接对它们进行操作。

【自动装箱，自动拆箱】在Java SE5中，为了减少开发人员的工作，Java提供了自动拆箱与自动装箱功能。

自动装箱：就是将基本数据类型自动转换成对应的包装类，一般是在基本类的内部定义了一个`valueOf`方法，其输入类型是基本类型（即int, char等），返回类型是封装类型（即Integer, Character等）

自动拆箱：就是将包装类自动转换成对应的基本数据类型，类中一般会定义一个方法，返回基本类型，如 `java.lang.Integer`中定义的`intValue`方法

自动装箱与自动拆箱都是由Java虚拟机加上的

【类继承】Java文件中非主类继承了一个不存在的类，IDE会提示错误。但如果并没有使用该类，则debug时不会影响运行

【类继承】继承类并重写方法时，必须保证输入参数列表以及**返回类型**相同。输入参数列表不同相当于重载，而输入参数类型相同但返回类型不同则会报错

【类继承】重写的方法的访问修饰符的限制一定要大于被重写方法的访问修饰符（public>protected>default>private）

【类继承】重写方法一定不能抛出新的检查异常或者比被父类方法申明更加宽泛的检查型异常。

> 例如：
>
> 父类的一个方法申明了一个检查异常 IOException，在重写这个方法是就不能抛出 Exception,只能抛出IOException 的子类异常，可以抛出非检查异常。

【类继承】重载的方法限制很少，但求输入参数列表不同。可以返回不同类型，抛出不同异常，不同访问限制类型

【类继承，接口】接口可以多继承，抽象类不行

【接口】接口中的方法默认为非静态方法，若声明为静态方法，则必须有方法体

【接口，修饰符】接口方法修饰符只能为 public, private, abstract, default, static, strictfp

【Java框架】Java 集合类框架的基本接口：Collection/ List / Set/ Map，其中 Map 和 List 都继承了 Collection

【失败】快速失败（fail-fast）与安全失败（fail-safe）。对于Java集合类而言，快速失败概括来说意思是：在对集合利用 Java.util.Iterator.iterator() 遍历时，如果修改了集合，则会报错，而利用 Java.util.concurrent.Iterator() 则会安全失败，即iterator遍历的始终是修改前的内容。区别是因为concurrent包中的iterator是对集合对象的拷贝进行的操作而util.iterator是直接在对象的内容上修改。参考：[面试官：说说快速失败和安全失败是什么 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/49148823)

【HashMap】调用 put 方法时，如果 key已经存在了，value 会被更新成新值

【基础语法】方法的传入参数为 `类型名 ... 参数名`时，代表可变长参数，相当于一个数组。并且，如果参数名指向一个数组，方法将可以修改数组的元素内容

【基础语法】|，& 既可以做位运算符，分别表示按位或，按位与，也可以做非短路逻辑运算符，即，其两侧的表达式均会被执行求值

【基础语法】位运算符：^ 异或，~ 非，>> 以符号位填充的右移，>>> 左侧补零无区别对待符号位右移，<< 不分符号位低位补零左移

【基础语法】单独的 T 代表一个类型 ，而 Class<T>代表这个类型所对应的类， Class<?>表示类型不确定的类

【垃圾收集】如果对象的引用被置为 null，垃圾收集器并不会立即回收内存，而是在下一个回收周期回收

【异常处理】异常处理完成以后，Exception 对象会在下一个垃圾回收过程中被回收掉

【基础语法】Java 中，== 运算符表示判断两个对象是否引用了同一个对象，对象的`equals`方法判断两个对象的内容是否相同。对于 String 类，只要不是用 new 运算符新建的对象，只要内容相同，都会指向相同的对象。

```java
String a = "abc";
String b = "ab" + "c";
String c = new String("abc");
// a == b 返回true
// a == c 返回false
// b == c 返回false
// a.equals(b),b.equals(c),a.equals(c)均返回true
```

【静态内部类和内部类的区别】静态内部类可以直接实例化，而内部类必须通过一个外部类的对象来实例化

```java
Outclass outobj = new Outclass();
Outclass.Innerclass obj = outobj.new Innerclass();
Outclass.StaticInnerClass sobj = new Outclass.StaticInnerClass();
```

【Java 内存】通常我们定义一个基本数据类型的变量，一个对象的引用，还有就是函数调用的现场保存都使用内存中的栈空间；而通过 new 关键字和构造器创建的对象放在堆空间；程序中的字面量（literal）如直接书写的 100、“hello”和常量都 是放在静态存储区中。

【方法，多态】静态方法被重写时，编译器不会报错，程序也可以运行，但是调用的时候运行的是n哪个方法（有待验证）

【接口，继承】抽象类可以实现(implements)接口，抽象类可继承具 体类，但前提是具体类必须有明确的构造函数

【继承】父类没有空参构造，而子类在构造函数的第一句又会默认调用父类的空参构造即隐身的super()，则此时编译不能通过

【位运算，易出bug，基础语法】根据java规范中描述，对于整型 int 进行移位操作，右操作数（移位的数量b）, b这个**操作数只能取二进制数的低五位（就是最后5位）**。也就是说，整型最多只会移动31位。

【类加载】类加载的时机

1. new，getstatic, putstatic,invokestatic 这四条字节码指令执行时，若未初始化类，则会先触发初始化。
2. 类未被加载，并对类进行反射调用
3. 其父类未初始化，会先初始化其父类（接口初始化时，并不会先初始化其父类）
4. 虚拟机启动时初始化含main方法的类
5. 当使用 JDK 1.7 的动态语言支持时，如果一个 java.lang.invoke.MethodHandle 实例最后的解析结果 REF_getStatic、REF_putStatic、REF_invokeStatic 的方法句柄，并且这个方法句柄所对应的类没有进行过初始化，则需要先触发其初始化。

【泛型编程】如果想在静态方法中使用泛型，必须在函数定义前加上泛型标志，且泛型标志必须在`static`与修饰符关键字之后

```java
访问类型修饰符 static <T> 返回类型 functionName(参数)
```

【JDK Collection】toArray 方法是在Collection接口中定义的

【Java技巧，JDK】`@SuppressWarnings`注解用于抑制编译器产生警告，可以加上一些参数，参考：[注解用法详解——@SuppressWarnings ](https://www.cnblogs.com/fsjohnhuang/p/4040785.html)

【Java特性，Java8】Java8新增了：Lambda表达式，方法引用，默认方法（接口中实现）Optional类, etc.

【浮点数】Java中的浮点数（float，double）定义了正无穷，负无穷和非数（NaN）。Float, Double 类中都有相应定义

【编程技巧】方法中如果需要对某个非final类型的引用指向的对象进行操作，但又不希望改变引用的值，可以先把引用的值赋给另一个变量，再利用该变量对对象进行访问

【继承】父类中的成员变量，只要是被声明为public 或 protected 或 子类与其在同一包中的，都可以被子类访问

【JDK内库，ArrayList】ArrayList中的size属性，并不等于ArrayList的elementData的长度。应该这么理解size：站在用户的角度而言，size代表用户当前存放在ArrayList中的元素数目。

【语言细节】i++ 和 ++i 的区别

1. i++：从局部变量表取出 i 并压入操作栈(load memory)，然后对局部变量表中的 i 自增 1(add&store memory)，将操作栈栈顶值取出使用，如此线程从操作栈读到的是自增之前的值。
2. ++i：先对局部变量表的 i 自增 1(load memory&add&store memory)，然后取出并压入操作栈(load memory)，再将操作栈栈顶值取出使用，线程从操作栈读到的是自增之后的值。

之前之所以说 i++ 不是原子操作，即使使用 volatile 修饰也不是线程安全，就是因为，可能 i 被从局部变量表（内存）取出，压入操作栈（寄存器），操作栈中自增，使用栈顶值更新局部变量表（寄存器更新写入内存），其中分为 3 步，volatile 保证可见性，保证每次从局部变量表读取的都是最新的值，但可能这 3 步可能被另一个线程的 3 步打断，产生数据互相覆盖问题，从而导致 i 的值比预期的小。

【特殊语法】`::`在Java中可用于调用类的静态方法，动态方法和构造函数

【特殊语法，Java14新特性】`instanceof`模式匹配

```java
// example
// 这是 Java 14 的新特性，判断obj的类型，若判断成功，直接生成一个String 类型的对象str，这个str对象作用域仅限于当前代码块
if (obj instanceof String str) {
	// code
}
```

【特殊语法，Java10新特性】`var`类型推断

```java
var a = "abc"; // 自动推断出a为字符串
var b = 1; // 自动推断出b为整型
```

【特殊语法，资源管理】Java 6 及以前，打开的资源必须手动关闭，也就是常见的 `try-catch-finally`，Java 7 开始，资源可以直接在 `try()`里进行声明构造，执行完`try`语句后，资源自动关闭。Java 9 开始，写法进一步简洁，`try()`里面可以是一个或多个变量（用分号`;`分隔），但必须是final的或者等同final（比如方法中声明的局部变量）

```java
// Java 7
try (MyInputStream mis = new MyInputStream(); MyOutputStream mos = new MyOutputStream() ) {
	mis.read("1.9");
	mos.write("1.9");
} catch (Exception e) {
	e.printStackTrace();
}
// Java 9
MyInputStream mis = new MyInputStream();
MyOutputStream mos = new MyOutputStream();
try (mis; mos) {
	mis.read("1.9");
	mos.write("1.9");
} catch (Exception e) {
	e.printStackTrace();
}
```

【修饰符】`strictfp`修饰符。被修饰符修饰的代码（类、接口、方法）中，所有的浮点数运算都严格遵守FP-strict的限制,符合IEEE-754规范，可以避免不同平台的误差

【特殊语法】静态引入`import static packageName`是Java 1.5新推出的特性。可以在不加类名的情况下访问类中的静态变量、静态方法以及类中定义的 public 的类，枚举等。如果静态引入导致了重名的方法或变量，会发生报错。

【Java内库，Integer】`IntegerCache`是`Integer`类中定义的一个静态类，被 `Integer.valueOf`方法使用，也就是说自动装箱会用到`IntegerCache`。同时，这也意味着，通过自动装箱创建两个Integer对象，只要值在IntergerCache范围之内，那么这两个对象其实是同一个，而如果是调用Integer的构造函数（）手动创建的两个对象，则即使二者值相等，不管在不在IntegerCache范围内，都是不同的对象。

【断言】Java 也具有断言机制

```java
assert expression [: error message];
// [] 代表是可选项，不是必须有的部分，error message是当断言失败时输出的错误信息
```

当断言中的表达式不满足时，系统会抛出`AssertationError`并提示相应错误

但是，只有当Java虚拟机开启断言选项时，断言语句才有作用

```bash
java -enableassertions <classname.java>
java -ea <classname.java>
```



## 待学习

【特殊语法，函数式编程】参考 [Java8新特性学习-函数式编程(Stream/Function/Optional/Consumer)](https://blog.csdn.net/icarusliu/article/details/79495534)

