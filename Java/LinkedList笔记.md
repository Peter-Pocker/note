Java JDK定义的LinkedList类，实际上是一个双向链表，其结点定义为一个静态内部类

```java
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

public boolean add(E e)

实际上是基于linkLast方法，也就是采用尾插法，从链表尾插入

public void addFirst(E e)

从头插入

public void addLast(E e)

public E removeLast()

public E removeFirst()

void linkLast(E e)

private void linkFirst(E e)

public void clear()

