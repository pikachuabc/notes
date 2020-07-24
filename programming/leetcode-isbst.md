# LeetCode:IsBST

{% embed url="https://leetcode.com/problems/binary-tree-level-order-traversal-ii/submissions/" %}

判断是否二叉搜索树（大小顺序为左根右）

自己的解法和高赞老哥的解法差不多，但是明显人家写的简洁。。不献丑了

```java
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean isValidBST(TreeNode root, long minVal, long maxVal) {
        if (root == null) return true;
        if (root.val >= maxVal || root.val <= minVal) return false;
        return isValidBST(root.left, minVal, root.val) && isValidBST(root.right, root.val, maxVal);
    }
```

总的思路还是递归遍历二叉树，关键在于判断左右孩子的值

{% hint style="info" %}
左/右子树的所有node都要小于/大于当前node值，单单判断每个node及其左孩子是不够的！
{% endhint %}

上面的问题在解法中以传入参数的方式巧妙（我觉得挺巧妙的也可能我太菜了）的解决

```java
public boolean isValidBST(TreeNode root, long minVal, long maxVal)
```

当该节点作为⬅️孩子时，有其上限为父节点的值。但是要注意这个值也有下限，_**因为我们不知道这个节点是否处于某个父节点的右侧，**_通过添加minVal这个参数即可解决

