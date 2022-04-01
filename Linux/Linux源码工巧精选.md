Linux源代码中广泛使用双向链表`struct list_head`，并嵌入在各种数据结构中，实现将其他数据结构以链表形式组织起来的功能。

list_head 可以嵌入进数据结构中的任何位置，并且可以定义任意多个。

list_head 是如何通过自身访问到所嵌入的结构中的其他成员的呢？

container_of

背后基于 typeof 编译器关键字与 offset 宏

[(33条消息) 详解container_of宏_你系得嘎，加油-CSDN博客](https://blog.csdn.net/yong199105140/article/details/8234071)

offsetof 是一个真正的宏，它定义在内核源代码[ include/linux/stddef.h](http://lxr.linux.no/linux+v2.6.29/include/linux/stddef.h#L24)文件中：

```c
#define offsetof(TYPE, MEMBER)((size_t) &((TYPE *)0)->MEMBER)
```

