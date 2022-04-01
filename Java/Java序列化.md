#### 什么是序列化？

将对象作为二进制流存储在外存的文件中，可以通过二进制文件重建对象

`Serializable`序列化接口没有任何方法或者字段，只是用于标识可序列化的语义。实现了 Serializable 接口的类可以被 `ObjectOutputStream `转换为字节流，同时也可以通过 `ObjectInputStream` 再将其解析为对象。而**静态变量是不能被初始化的**

`transient` 修饰符

1. 一旦变量被transient修饰，变量将不再是对象持久化的一部分，该变量内容在序列化后无法获得访问。

2. transient关键字只能修饰变量，而不能修饰方法和类。注意，本地变量是不能被transient关键字修饰的。变量如果是用户自定义类变量，则该类需要实现Serializable接口。

3. 被transient关键字修饰的变量不再能被序列化，一个静态变量不管是否被transient修饰，均不能被序列化。

transient 原理

序列化的对象包含被 `transient` 修饰的实例变量时，java 虚拟机(JVM)跳过该特定的变量。

该修饰符包含在定义变量的语句中，用来预处理类和变量的数据类型。

被 transient修饰并非一定不能被序列化。在Java中，对象的序列化可以通过实现两种接口来实现，若实现的是`Serializable`接口，则所有的序列化将会自动进行，若实现的是`Externalizable`接口，则没有任何东西可以自动序列化，需要在`writeExternal`方法中进行手工指定所要序列化的变量，这与是否被transient修饰无关。