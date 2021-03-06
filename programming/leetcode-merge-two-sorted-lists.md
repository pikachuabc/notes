# LeetCode:merge-two-sorted-lists

{% embed url="https://leetcode.com/problems/merge-two-sorted-lists/" %}

合并两个排序过的链表，我发现链表这块的问题。。思路都有但是会碰到很多空指针的问题，以后注意。

## 自己的暴力解法

```java
ArrayList<ListNode> result = new ArrayList<>();
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    iterate(l1);
    iterate(l2);
    if (result.size() == 0) {
        return null;
    }
    result.sort((o1, o2) -> {
        if (o1.val >= o2.val) {
            return 1;
        } else {
            return -1;
        }
    });
    for (int i = 0; i < result.size()-1; i++) {
        result.get(i).next = result.get(i+1);
    }
    result.get(result.size()-1).next = null;
    return result.get(0);
}

private void iterate(ListNode node) {
    while (node != null) {
        result.add(node);
        node = node.next;
    }
}
```

 将两个链表所有值放在Array里面然后排序，最后用这个Array构建新的链表

```java
result.get(result.size()-1).next = null;
```

这里注意一下要将链表尾节点的next清除

## 看似优美的递归解法

用debug跑了一次才看懂这个解法。。

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2){
    if(l1 == null) return l2;
    if(l2 == null) return l1;
    if(l1.val < l2.val){
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    } else{
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
}
```

 其实也好理解这个递归，每次从两个链表中选择数值较小的节点👇，return值的含义是上一个节点应该接的下一个节点，如果某一个链表到了null直接把剩下的链表接回去，减少循环（完爆自己写的排序）。

```java
if(l1.val < l2.val){
        ....
    } else{
        ....
    }
```

将这个点作为最终结果的一个节点，然后将剩下的链表节点继续递归，一开始没有看懂的原因在于对于节点的连接是反着来的（将节点n赋值到n-1.next上一直到头节点，和递归函数返回的顺序一样）  
但是对于递归来说。。如果链表的长度变得很长的话对于内存的占用也很高，容易出现内存泄漏。

