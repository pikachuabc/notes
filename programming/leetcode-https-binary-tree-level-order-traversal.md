# LeetCode: https:binary-tree-level-order-traversal

{% embed url="https://leetcode-cn.com/problems/binary-tree-level-order-traversal/" %}

用前序遍历做，easy

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

思路比较好说，遍历（pre-order）的同时记录节点的层数信息最后输出，这里比较值得注意的是⬇️

```java
if (height >= res.size()) {
        res.add(new LinkedList<>());
    }
```

这部分应该不算算法的问题了。implement的时候发现没办法判断某个节点所在的层的list是否已经存在（不存在就new一个）,会报空指针异常。。上面这个解决方法还挺巧妙的

## Related question

{% embed url="https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/submissions/" %}

还有几道相似的问题一并放在这里，要求以拉链的方式输出每层元素，思路和上面的相同，先层序遍历然后反转奇数层就可以，这个是我第一个写出来用时和空间都99%解法= =，纪念一下

```java
List<List<Integer>> result = new ArrayList<>();
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    iterate(root,0);
    for (int i = 0; i < result.size(); i++) {
        //如果是奇数层做反转
        if ((i & 1) == 1) {
            reverse(result.get(i));
        }
    }
    return result;
}

public void iterate(TreeNode node, int height) {
    if (node==null)return;
    if (height >= result.size()) {
        result.add(new ArrayList<>());
    }
    result.get(height).add(node.val);
    iterate(node.left,height+1);
    iterate(node.right,height+1);
}

public void reverse(List<Integer> arrayList) {
    int i = 0, j = arrayList.size()-1;
    while (j >= i) {
        int temp= arrayList.get(i);
        arrayList.set(i,arrayList.get(j));
        arrayList.set(j,temp);
        i++;
        j--;
    }
}
```

奇偶层判断的时候用位操作可以节省空间占用

```java
if ((i & 1) == 1) {
            reverse(result.get(i));
        }
```

