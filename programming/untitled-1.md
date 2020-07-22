---
description: LeetCode 二叉树最大路径和
---

# LeetCode:binary-tree-maximum-path-sum

## 在经历了两个小时的包括躺着坐着蹲着的思考解决了这个问题并且给出了一个非常不elegant的解法（深刻认识到了自己的水平），这里分析一下大佬的解法与思路，保证看懂。

题目要求找出二叉树路径的最大和（至少包含一个节点，并且要求是_**path，不是连通区域**_），二叉树定义如下

```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode() {
    }

    TreeNode(int val) {
        this.val = val;
    }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

### divide and conquer

一眼看到这个题想起了硬币凑钱的问题，借鉴了一些里面的思想进行解释，先看整体思路

```java
public int maxPathDown(TreeNode node) {
        if (node == null) return 0;
        int left = Math.max(0, maxPathDown(node.left));
        int right = Math.max(0, maxPathDown(node.right));
        maxValue = Math.max(maxValue, left + right + node.val);
        return Math.max(left, right) + node.val;
    }
```

{% hint style="info" %}
对于每一个节点，它要么是这个路径的根节点（最高那个），要么不是
{% endhint %}

以是否为根节点进行divide and conquer的原因在于path是否能**同时**包含左右子二叉树，以这种方式递归遍历所有节点计算该节点作为根节点时候的路径和，能得到答案，下面解释一下每句的含义，第一句node判断null做递归用相当于前序遍历

```java
int left = Math.max(0, maxPathDown(node.left));
```

这里和0做比较的意义在于如果左或者右子二叉树中的最大路径和是负数，那么宁愿舍弃掉这一半（视作0）

```java
maxValue = Math.max(maxValue, left + right + node.val);
```

maxValue是一个全局变量用来记录最大的路径和，后面 left+right+node.val表示如果该节点作为根节点能够达到的最大路径和，实际上这里根据left，right是否为0包含了三种情况。根，根左，根右。这是divide and conquer的第一部分。

```java
return Math.max(left, right) + node.val;
```

最后这句就比较精髓了，在该节点不是根节点的情况下，将该节点作为根节点的最大路径和返回（相当于把这部分子树压缩在了一个节点中，节点的值是这部分树的最大路径和），由于题目要求是路径，所以只能选左或者右子树压缩，选择大的那个。

## Out of box

如果要求路径可达区域的最大和（并非一串），稍加修改返回，left+right即可

如果非二叉树，即是经典的[Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)

