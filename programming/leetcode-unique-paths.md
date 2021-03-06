# LeetCode:Unique Paths

{% embed url="https://leetcode-cn.com/problems/unique-paths/submissions/" %}

好说，经典DP，维护一个mn数组就可以

```java
public int uniquePaths(int m, int n) {
    int[][] grid = new int[n][m];
    for (int i = 0; i < n; i++) {
        grid[i][0] = 1;
    }
    for (int i = 0; i < m; i++) {
        grid[0][i] = 1;
    }
    for (int i = 1; i < n; i++) {
        for (int j = 1; j < m; j++) {
            grid[i][j] = grid[i - 1][j] + grid[i][j - 1];
        }
    }
    return grid[n - 1][m - 1];
}
```

 其实只需要维护一行就可以

```java
public int uniquePaths1(int m, int n) {
    int[] grid = new int[m];
    Arrays.fill(grid, 1);
    for (int j = 1; j < n; j++) {
        for (int i = 1; i < m; i++) {
            grid[i] = grid[i] + grid[i - 1];
        }
    }
    return grid[m - 1];
}
```

 其实可以用分治的方法做，不过数组大的话会超时

```java
public int uniquePaths2(int m, int n) {
    if (m == 1 || n == 1) {
        return 1;
    }
    return uniquePaths2(m - 1, n) + uniquePaths2(m, n - 1);
}
```

 



