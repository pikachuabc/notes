# LeetCode: Largest Rectangle in Histogram

{% embed url="https://leetcode.com/problems/largest-rectangle-in-histogram/" %}

另一个让我见识到大佬思路的题目，这个和之前的水箱问题类似但是又不同

{% page-ref page="leetcode-water-tank.md" %}

原来打算用双指针的方法做结果发现其实不太行，在水箱问题中移动左右指针的条件是判断哪边小，在这个题目中是不适用的因为面积的高度是取决于这两个边界之间的最低高度无法判断移动哪边的指针。

## Brute force

```java
 public int largestRectangleArea(int[] heights) {
    if (heights.length==0) {
        return 0;
    }
    int max = 0;
    for (int i = 0; i < heights.length-1; i++) {
        int height = heights[i];
        for (int j = i ; j < heights.length; j++) {
            height = Math.min(height,heights[j]);
            max = Math.max(max,height*(j-i+1));
        }
    }
    if (heights[heights.length - 1] > max) {
        max = heights[heights.length-1];
    }
    return max;
}
```

老规矩先暴力解法，这个倒是和水箱那个差不多，不同点在于中间的height要遍历两个边界的所有高度选最低那个

## stack

这个解法着实让我觉得神奇

```java
public int largestRectangleArea(int[] heights) {
        int len = heights.length;
        Stack<Integer> s = new Stack<>();
        int maxArea = 0;
        for (int i = 0; i <= len; i++){
            int h = (i == len ? 0 : heights[i]);
            if (s.isEmpty() || h >= heights[s.peek()]) {
                s.push(i);
            } else {
                int tp = s.pop();
                maxArea = Math.max(maxArea, heights[tp] * (s.isEmpty() ? i : i - 1 - s.peek()));
                i--;
            }
        }
        return maxArea;
    }
```

先说一下大致思路，以每个数组中的数字作为高度，确定左右边界（左边右边第一个小雨这个高度的就是边界），然后比大小得出答案，这个方法的神奇之处在于确定左右边界上，具体来说是这个部分

```java
 heights[tp] * (s.isEmpty() ? i : i - 1 - s.peek())
```

对于tp这个index，堆栈（堆栈中是数组的index而不是具体的值）中最上面的index是位于tp左边的第一个heights\[index\]小于heights\[tp\]得index（如果比tp大的话那么肯定会被先pop掉作为tp，所以不可能），所以左边界的index是s.peek\(\)+1，然后循环中这个i是位于tp右第一个heights\[index\]小于heights\[tp\]的index，因为👇如果大于当前heights\[tp\]（注意这里的tp代表堆栈顶端，由s.peek\(\)获得的）那么当前的i会被压进s，所i-\(s.peek\(\)+1\)就是左右边界的差值

```java
if (s.isEmpty() || h >= heights[s.peek()])
```

最后i--也很重要，因为当前的height\[i\]是用做计算面积了，需要还原，不然结果会缺失以heights\[i\]为高度的面积比较

