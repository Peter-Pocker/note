hashmap中的每一项都是一个Entry，即一个键值对，每个键值对（Entry）有一个32位的hash值。

```Java
/* 源代码 java.util.HashMap.java中定义，省略部分方法定义 */
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) 

        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }

        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }
/* java.util.Objects.java 中定义的 hashCode 方法
	public static int hashCode(Object o) {
        return o != null ? o.hashCode() : 0;
    }
*/
        public final V setValue(V newValue)

        public final boolean equals(Object o)
}
```

hashmap默认填充比率是0.75

哈希表数组的最大长度是2的30次方

hashmap中的哈希查找

```java
 /* get方法其实是调用getNode方法 */
	final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            /* n是2都幂，那么n一定是一个只有1个1的32位整型，那么n-1必定是有一串全1，并且这串全1是从最低位开始的。并且1的个数一定是表table的长度 */
            (first = tab[(n - 1) & hash]) != null) {
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            if ((e = first.next) != null) {
                
                if (first instanceof TreeNode)
                    /* TreeNode结点这一块我暂时没看懂 */
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
```

向哈希表中加入元素

```java
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            /* 发生冲突 */
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount; // 用于实现快速失败
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```



问题

- [ ] 静态方法 `compatableClassFor(Object x)`没读懂
- [ ] HashMap对象拥有table成员，定义时修饰符是空白（即default），可是为什么在运行时不能访问呢？这是怎么做到的？

