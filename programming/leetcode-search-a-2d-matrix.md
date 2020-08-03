# LeetCode:search-a-2d-matrix

{% embed url="https://leetcode-cn.com/problems/search-a-2d-matrix/submissions/" %}

题目要求在排序过的二维数组中寻找一个数字，这个题算是二分插入和二分查找的结合了，二分这块主要是边界问题，思路很简单先在数组上二分插入寻找在哪个一维数组中然后再用二分查找

```java
public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix.length==0|| matrix[0].length==0|| target < matrix[0][0] || target > matrix[matrix.length - 1][matrix[matrix.length - 1].length-1]) {
        return false;
    }
    int groupIndex = matrix.length - 1;
    int left = 0;
    int right = matrix.length - 1;
    while (left <= right) {
        int mid = ((right - left) >> 1) + left;
        if (target < matrix[mid][0]) {
            groupIndex = mid - 1;
            right = mid - 1;
        } else if (target > matrix[mid][0]) {
            left = mid + 1;
        } else {
            return true;
        }
    }

    left = 0;
    right = matrix[groupIndex].length - 1;
    while (left <= right) {
        int mid = ((right - left) >> 1) + left;
        if (target < matrix[groupIndex][mid]) {
            right = mid - 1;
        } else if (target > matrix[groupIndex][mid]) {
            left = mid + 1;
        } else {
            return true;
        }
    }
    return false;

}
```

