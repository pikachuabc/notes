# LeetCode: partition-list

{% embed url="https://leetcode.com/problems/partition-list/" %}

没做出来= =看了答案之后发现自己是shabi。看见partition后首先想起了quicksort，忽视了题中要求int x要保持在原位置。。。

```java
public ListNode partition(ListNode head, int x) {

    // before and after are the two pointers used to create the two list
    // before_head and after_head are used to save the heads of the two lists.
    // All of these are initialized with the dummy nodes created.
    ListNode before_head = new ListNode(0);
    ListNode before = before_head;
    ListNode after_head = new ListNode(0);
    ListNode after = after_head;

    while (head != null) {

        // If the original list node is lesser than the given x,
        // assign it to the before list.
        if (head.val < x) {
            before.next = head;
            before = before.next;
        } else {
            // If the original list node is greater or equal to the given x,
            // assign it to the after list.
            after.next = head;
            after = after.next;
        }

        // move ahead in the original list
        head = head.next;
    }

    // Last node of "after" list would also be ending node of the reformed list
    after.next = null;

    // Once all the nodes are correctly assigned to the two lists,
    // combine them to form a single list which would be returned.
    before.next = after_head.next;

    return before_head.next;
}
```

 思路比较清晰。。维护两个列表，小于partition value的链到第一个表上，**大于或者等于**的链到第二个列表上（由此达到了维持原partition位置不变的目的）



