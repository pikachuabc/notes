# LeetCode:lowest-common-ancestor-of-a-binary-tree​

{% embed url="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/" %}

求二叉树某两个节点的公共，位置最低的祖先（其中一个节点可以作为祖先）

## 自己的递归解法\(优化较少但是比较好懂\)

```java
TreeNode result = null;
    public boolean ifExist(TreeNode root, TreeNode p, TreeNode q) {

        if (result!=null||root==null){
            return false;
        }
        
        if (root.val == p.val || root.val == q.val) {
            boolean inLeft = ifExist(root.left,p,q);
            boolean inRight = ifExist(root.right,p,q);
            if (inLeft||inRight){
                result = root;
            }
            return true;
        }

        boolean inLeft = ifExist(root.left,p,q);
        boolean inRight = ifExist(root.right,p,q);
        if (inLeft&&inRight){
            result = root;
        }
        return inLeft|inRight;
    }
```

整体思路为：对于每一个节点，如果是两个节中一个，那么只需要判断该节点左右子树中是否包含另一个节点，包含的话答案即为这个节点，如果不是两个节点中的一个，那么判断这个节点是否包含两个节点中任意一个返回boolean。

{% hint style="info" %}
公共节点的特点是：如果公共节点不是两个节点的任意一个，那么这两个节点必定分别存在于该节点的左右子树中
{% endhint %}

有了👆的逻辑，就很容易implement了

```java
if (root.val == p.val || root.val == q.val)
```

如果该节点是两个节点中的一个，递归判断另一个节点是否在该节点下，如果在的话这个节点就是答案

```java
if (inLeft&&inRight){
            result = root;
        }
```

如果该节点不是两个节点中的一个，那么判断是否左右子树均含有目标节点

```java
if (result!=null||root==null){
            return false;
        }
```

这里做了一点小优化，如果已经得到了答案停止递归

## 再来看看高赞答案（递归&遍历）

### 递归

```java
private TreeNode ans;

public No1Ans() {
    // Variable to store LCA node.
    this.ans = null;
}

private boolean recurseTree(TreeNode currentNode, TreeNode p, TreeNode q) {

    // If reached the end of a branch, return false.
    if (currentNode == null) {
        return false;
    }
    // Left Recursion. If left recursion returns true, set left = 1 else 0
    int left = this.recurseTree(currentNode.left, p, q) ? 1 : 0;
    // Right Recursion
    int right = this.recurseTree(currentNode.right, p, q) ? 1 : 0;

    // If the current node is one of p or q
    int mid = (currentNode == p || currentNode == q) ? 1 : 0;
    
    // If any two of the flags left, right or mid become True
    if (mid + left + right >= 2) {
        this.ans = currentNode;
    }

    // Return true if any one of the three bool values is True.
    return (mid + left + right > 0);
}
```

这个思路其实和自己想的的差不多，不过作者很机智的简化了判断逻辑

```java
if (mid + left + right >= 2) {
        this.ans = currentNode;
    }
```

这里的mid代表是两个节点中的一个，left和right分别代表是存在于左右节点中，只要满足任意两个条件即可判断该节点就是答案

### 遍历

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

    // Stack for tree traversal
    Deque<TreeNode> stack = new ArrayDeque<>();

    // HashMap for parent pointers
    Map<TreeNode, TreeNode> parent = new HashMap<>();

    parent.put(root, null);
    stack.push(root);

    // Iterate until we find both the nodes p and q
    while (!parent.containsKey(p) || !parent.containsKey(q)) {

        TreeNode node = stack.pop();

        // While traversing the tree, keep saving the parent pointers.
        if (node.left != null) {
            parent.put(node.left, node);
            stack.push(node.left);
        }
        if (node.right != null) {
            parent.put(node.right, node);
            stack.push(node.right);
        }
    }

```

这个解法中作者用了stack进行二叉树的pre-order遍历，用HashMap记录每个节点的父节点当遍历到两个节点之后停止

```java
// Ancestors set() for node p.
    Set<TreeNode> ancestors = new HashSet<>();

    // Process all ancestors for node p using parent pointers.
    while (p != null) {
        ancestors.add(p);
        p = parent.get(p);
    }

    // The first ancestor of q which appears in
    // p's ancestor set() is their lowest common ancestor.
    while (!ancestors.contains(q))
        q = parent.get(q);
    return q;
    }
```

{% hint style="info" %}
每个节点向上到root的路径只有一条
{% endhint %}

这部分就很精髓了，先得到其中一个点到root的所有节点，再从第二个节点开始找，第一个出现的存在于第一个节点集的节点就是两者的最小祖先。clever

