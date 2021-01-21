# Collection & Map

![](../.gitbook/assets/image%20%283%29.png)

### ArrayList

动态数组实现，增加大小的话需要扩容，所以如果需要添加大量元素需要提前说明数组容量

线程不安全，需要借助如下构造方式达到目的

```java
List list = Collections.synchronizedList(new ArrayList(...));
```

### LinkedList

链表实现数组（双向链表），添加删除快。getset拉胯

线程不安全，同样可以通过上述方法解决

![](../.gitbook/assets/image%20%284%29.png)

### Map & Table

hashmap和table都实现了Map接口，hashMap线程不安全（在resize的过程中如果多个线程同时resize会导致loop），不允许key为null，hashmap是table之后出现的，允许null. Hashtable虽然安全但是每次会锁住整个表，效率低，所以有了concurrentHashMap

map数量超过8个链表就转为红黑树结构

