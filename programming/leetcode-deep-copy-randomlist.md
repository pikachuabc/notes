# LeetCode:deep-copy-RandomList

{% embed url="https://leetcode-cn.com/problems/copy-list-with-random-pointer/" %}

要求dee copy一个含有随机指针的链表，看似简单实际上需要一些思路，用map和array

的方法都做了一遍

### Array

```java
public Node copyRandomList(Node head) {
    ArrayList<Node> temp = new ArrayList<>();
    if (head == null) {
        return null;
    }
    while (head != null) {
        temp.add(head);
        head = head.next;
    }
    Node[] result = new Node[temp.size()];
    //handle value
    for (int i = 0; i < result.length; i++) {
        result[i] = new Node(temp.get(i).val);
    }
    //handle next
    for (int i = 0; i < result.length-1; i++) {
        result[i].next = result[i+1];
    }
    //handle random
    for (int i = 0; i < result.length; i++) {
        int index = temp.indexOf(temp.get(i).random);
        if (index != -1) {
            result[i].random = result[index];
        }
    }
    return result[0];

```

这个题目需要思考的问题是如何new出新节后其中的random指针能指到同样是copy的新节点，👆的思路中先用arraylist遍历节点后new出一组数值，位置完全相同的array，在random的连接部分👇index代表原始节点中某个节点指向的新节点在数组中的位置（如果是null的话indexOf会返回-1，这个时候就leave it as null就好），这个位置可以映射到新的数组中做链接用。

```java
for (int i = 0; i < result.length; i++) {
        int index = temp.indexOf(temp.get(i).random);
        if (index != -1) {
            result[i].random = result[index];
        }
    }
```

### Map

```java
public Node copyRandomList(Node head) {
    if (head == null) return null;

    Map<Node, Node> map = new HashMap<Node, Node>();

    // loop 1. copy all the nodes
    Node node = head;
    while (node != null) {
        map.put(node, new Node(node.val));
        node = node.next;
    }

    // loop 2. assign next and random pointers
    node = head;
    while (node != null) {
        map.get(node).next = map.get(node.next);
        map.get(node).random = map.get(node.random);
        node = node.next;
    }

    return map.get(head);
}
```

其实和array的思路是一样的，只要deep copy的节点能和原先的节对应上就能解决指针的问题

