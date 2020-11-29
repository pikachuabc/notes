# LeetCode: max Perimeter

给一个数组，求最大周长的三角形

```java
public int largestPerimeter(int[] A) {
        Arrays.sort(A);
        for (int i = A.length - 1; i >= 2; --i) {
            if (A[i - 2] + A[i - 1] > A[i]) {
                return A[i - 2] + A[i - 1] + A[i];
            }
        }
        return 0;
    }
```

没啥说的，先排序保证nlogn的复杂度然后判断三边关系，A+B&gt;C

