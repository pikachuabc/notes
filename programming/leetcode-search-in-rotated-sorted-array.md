# LeetCode:search-in-rotated-sorted-array

{% embed url="https://leetcode-cn.com/problems/search-in-rotated-sorted-array/" %}

这个题分1和2部分，第二部分就是说数组中可以存在重复的值，其实就是之前的题的综合

{% page-ref page="leetcode-find-minimum-in-rotated-sorted-array.md" %}

## 不包含重复元素

第一部分找到最小的值（这个最小值把数组分成两部分，看target在哪一部分再进行二本查找就可以）

```java
public int search1(int[] nums, int target) {
    if (nums.length == 0) {
        return -1;
    }
    //find mini element index
    int left = 0;
    int right = nums.length - 1;
    while (left < right) {
        int mid = ((right - left) >> 1) + left;
        if (nums[mid] > nums[right]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    //binSearch
    return target > nums[nums.length - 1] ?
            binSearch(nums, 0, left - 1, target) :
            binSearch(nums, left, nums.length - 1, target);

}

private int binSearch(int[] nums, int left, int right, int target) {
        System.out.println("search from " + left + " to " + right);
        while (left <= right) {
            int mid = ((right - left) >> 1) + left;
            if (nums[mid] == target) {
                return mid;
            } else if (target < nums[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return -1;
    }
```

 

## 存在重复部分

本来想直接用之前题解

```java
public boolean search(int[] nums, int target) {
    if (nums.length == 0) return false;
    int left = 0;
    int right = nums.length - 1;
    while (left < right) {
        int mid = ((right - left) >> 1) + left;
        if (nums[mid] > nums[right]) {
            left = mid + 1;
        } else if (nums[mid] < nums[left]) {
            right = mid;
        } else {
            right--;
        }
    }
    int result = target > nums[nums.length - 1] ?
            binSearch(nums, 0, left - 1, target) :
            binSearch(nums, left, nums.length - 1, target);
    return result != -1;
}
```

 看似很美好，但是= =，当测试\[1,1,1,3,1\], target =3 这个例子的时候，👆的程序会返回false，这里的程序是**找到数组中的最小值而不是排序数组的开头，**在没有重复元素的时候这两个问题是等价的，当存在重复元素并且重复元素是头元素的时候，最小元素的index=left得到的是第一个重复元素的位置，对于上面的例子就是0，用这个位置去分搜索部分显然是不对的

plan1：这种情况出现的情况就是首尾元素重复，并且最小的值还就是重复的值，对于这种情况单独拿出来用其他非排序的搜索方法

plan2：考虑所有情况，我没有做出来因为感觉有点复杂于是看了答案，推荐这个老哥的分析，每种情况都说到了

[https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/solution/tong-guo-hua-tu-lai-shen-ke-li-jie-er-fen-fa-by--2/](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/solution/tong-guo-hua-tu-lai-shen-ke-li-jie-er-fen-fa-by--2/)



