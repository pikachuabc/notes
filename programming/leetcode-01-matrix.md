# LeetCode:01-matrix

[https://leetcode.com/problems/01-matrix/submissions/](https://leetcode.com/problems/01-matrix/submissions/)

说来惭愧medium的题做了一下午，不过中间又复习图的遍历以及动规的一切问题也算有所收获，总结下，题目要求求一个矩阵中每个1离0的最近距离，方向可选为上下左右（注意可以拐弯。。比如左上左下等，第一次傻呵呵只考虑了上下左右的距离）

## DP

```java
public int[][] updateMatrix(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            matrix[i][j] = matrix[i][j] == 0 ? 0 : 10000;
        }
    }

    // up solution
    for (int j = 0; j < n; j++)  {
        for (int i = 0; i < m; i++){
            if (i - 1 >= 0) {
                matrix[i][j] = Math.min(matrix[i][j], matrix[i - 1][j] + 1);
            }
        }
    }

    // down solution
    for (int i = m - 1; i >= 0; i--) {
        for (int j = n - 1; j >= 0; j--) {
            if (i + 1 < m) {
                matrix[i][j] = Math.min(matrix[i][j], matrix[i + 1][j] + 1);
            }
        }
    }
    //left solution
    for (int j = 0; j < n; j++) {
        for (int i = 0; i < m; i++) {
            if (j - 1 >= 0) {
                matrix[i][j] = Math.min(matrix[i][j], matrix[i][j - 1] + 1);
            }
        }
    }
    //right solution
    for (int j = n - 1; j >= 0; j--) {
        for (int i = m - 1; i >= 0; i--) {
            if (j + 1 < n) {
                matrix[i][j] = Math.min(matrix[i][j], matrix[i][j + 1] + 1);
            }
        }
    }
    return matrix;
}
```

看答案学会的（确实不会），顺复习了下动归的知识，对于每个非0点来说，0到它的最短距离等于1+上下左右的四个点中到0最短的那个距离，DP的思想在于实时根据上一个状态更新下一个状态（这里的状态就是指点到0的距离），这里我们需要四次更新，分别是当最短到0路径分别来自上下左右的情况（其实可以简化到两次，左上👉右下或右上👉左下，dp要保证更新顺序的根据是来自该轮更新顺序下已经更新过的点，这样才更新的意义）。

## BFS

```java
public int[][] updateMatrix(int[][] matrix) {
    // 首先将所有的 0 都入队，并且将 1 的位置设置成 -1，表示该位置是 未被访问过的 1
    Queue<int[]> queue = new LinkedList<>();
    int m = matrix.length, n = matrix[0].length;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (matrix[i][j] == 0) {
                queue.offer(new int[] {i, j});
            } else {
                matrix[i][j] = -1;
            }
        }
    }

    int[] dx = new int[] {-1, 1, 0, 0};
    int[] dy = new int[] {0, 0, -1, 1};
    while (!queue.isEmpty()) {
        int[] point = queue.poll();
        int x = point[0], y = point[1];
        for (int i = 0; i < 4; i++) {
            int newX = x + dx[i];
            int newY = y + dy[i];
            // 如果四邻域的点是 -1，表示这个点是未被访问过的 1
            // 所以这个点到 0 的距离就可以更新成 matrix[x][y] + 1。
            if (newX >= 0 && newX < m && newY >= 0 && newY < n
                    && matrix[newX][newY] == -1) {
                matrix[newX][newY] = matrix[x][y] + 1;
                queue.offer(new int[] {newX, newY});
            }
        }
    }

    return matrix;
}
```

这里用了queue实现图的BFS，原点是所有的0点，将其index放进队列中，将所有的1点标记为为访问的点（-1）下面分析一下详细过程

```java
int[] dx = new int[] {-1, 1, 0, 0};
int[] dy = new int[] {0, 0, -1, 1};
```

👆这个是扩散的参数，下面的for循环中newX和newY分别加dx\[i\]和dy\[i\]表示扩散的方向依次为向上向下向左向右

```java
if (newX >= 0 && newX < m && newY >= 0 && newY < n
                    && matrix[newX][newY] == -1)
```

这个判前面几个都是判断边界防止越界，最后一个是判断是否该点为未访问过的节点（1节点），如果是的话更新到1的距离为该节点到0的距离（其实就是maxtrix\[x\]\[y\]，对于0节点来说就是0，对于访问过的1节点的话是该1节点到0的最近距离），之后将该1节点排进队列（否则就只能更新0节adjacent的1节点）



其实BFS这里感觉多少也有点DP的意思，只不过更新的顺序是以0节点为中心，1个节点长度为单位进行更新的

