# LeetCode: https:binary-tree-level-order-traversal

{% embed url="https://leetcode-cn.com/problems/binary-tree-level-order-traversal/" %}

层序遍历输出节点值，用DFS做，easy

```java
List<List<Integer>> res = new ArrayList<>();

public List<List<Integer>> levelOrder(TreeNode root) {
    iterate( root, 0);
    return res;
}

public void iterate(TreeNode root, int height) {
    if (root == null) return;
    if (height >= res.size()) {
        res.add(new LinkedList<>());
    }
    res.get(height).add(root.val);
    iterate(root.left, height+1);
    iterate(root.right, height+1);
}
```

思路比较好说，遍历（pre-order）的同时记录节点的层数信息最后输出

