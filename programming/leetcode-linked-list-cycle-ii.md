# LeetCode: Linked List Cycle II

{% embed url="https://leetcode.com/problems/linked-list-cycle-ii/discuss/44793/O\(n\)-solution-by-using-two-pointers-without-change-anything" %}

这个题做出来不是很难，但是双指针那个解法的思路着实有趣，拿来分享下

## 自己的解法

看到这种循环/重复元素的问题就会想到set...所以思路很清晰了所有节点都往set里面扔直到某次add失败说明有重复元素，这个元素就是循环的头节点，思路清晰但是时间，尤其是空间复杂度惨不忍睹

```java
public ListNode detectCycle(ListNode head) {
    Set<ListNode> set = new HashSet<>();

    while (head != null) {
        if (!set.add(head)) {
            return head;
        }
        head = head.next;
    }
    return null;
}
```

## 双指针解法

```java
public ListNode detectCycle(ListNode head) {
    if (head == null || head.next == null) return null;
    ListNode slow = head, fast = head, start = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            while (slow != start) {
                slow = slow.next;
                start = start.next;
            }
            return start;
        }
    }
    return null;
}
```

一下看不懂没关系，我看了好久才看懂= =这其实是一个数学问题：假设慢指针slow走过的路程（节点数量）是x，那么快指针走过的节点数量就是2x，假设这个循环的长度（包含的节点数）是l，当慢指针和快指针在循环中相遇的时候

$$
2x-x = n*l
$$

好了我来解释这个等式：假设我们的慢指针和快指针都是一次走完所有路程，那么当慢指针和快指针一起走了x的时候慢指针停下了（循环中的某个位置），快指针还有x的路程要走，此时慢指针和快指针是重合的，为了使得快指针走完剩下的x后仍然能和慢指针当前的位置重合，**那么x必须是n倍的循环长度**！！！所以我们得到

$$
x = n*l
$$

这个就很有意思了，x是什么，是链表头到相遇节点的距离，它等于n倍的循环的长度，它在这个**带有循环的链表中一直成立！！！**所以当我们同时向后移动一次头节点的指针和相遇节点的指针，这个距离关系仍然成立（即使相遇节点可能因为循环回到了前面的节点，这时候说明n变成了n-1）。

最精彩的地方来了：当从头节点出发的指针指到了循环中的第一个节点，**为了使得上述关系仍然成立，此时从相遇节点出发的指针必定也指向循环中的第一个节点，两个节点重合，此时n=0**，因为两个指针都指到了循环中的节点，而这两个节点的距离关系的范围是0～l-1。由此得出结论，一旦来自头节点的指针进入循环指向了循环的头节点，它必定和另一从一开始相遇节点出发的指针重合指向同一个节点（头节点），下面部分就是在干这个

```java
while (slow != start) {
                slow = slow.next;
                start = start.next;
            }
            return start;
```

精彩！

