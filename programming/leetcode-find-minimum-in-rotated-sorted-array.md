# LeetCode:find-minimum-in-rotated-sorted-array

{% embed url="https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/submissions/" %}

寻找一个翻转过的排序数组中的最小数字，一般这种提到了排序好了的数组问题都可以用二分查找来解决，这里其实主要考察了边界问题。。。在写出自己的第一版后发现很多边界都会出问题，下面分析一下

```java
public int findMin(int[] nums) {
    if (nums.length == 1 || nums[0] < nums[nums.length - 1]) {
        return nums[0];
    }
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        int mid = ((right - left) >> 1) + left;
        if (nums[mid] < nums[left]) {
            right = mid - 1;
        } else if (nums[mid] > nums[right]) {
            left = mid + 1;
        } else {
            return nums[left];
        }
    }
    return nums[0];
}
```

思路就是对于每次的中间元素，有三种情况，第一种这个数组没有被反转过，mid &gt; left && mid &lt; right, 这种情况在上面的if中已经排除过了，剩下两种情况分别是mid &lt; left （说明左边的数组不正常，包括了反转过后的部分，注意这里的左边其实是包含mid的，如果mid就是我们要找的开头最小值，那么除了mid的左半部分其实是正常的，会造成错误结果，就和上面的代码一样），另一种情况mid &gt; right（说明右边的数组不正常，这里就可以不用考虑mid了因为最小值一定在右侧），第一种情况的问题也好解决，加一个判断mid值是否是最小的就可以

```java
if (mid != 0 && nums[mid] < nums[mid - 1]) {
    return nums[mid];
}
```

再放一个简单的处理边界值的解法

```java
public int findMin2(int[] nums) {
    int left = 0;
    int right = nums.length - 1;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] > nums[right]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }

    return nums[left];
}
```

### 允许重复元素出现

```java
public int findMin3(int[] nums) {
    int low = 0;
    int high = nums.length - 1;
    while (low < high) {
        int pivot = low + (high - low) / 2;
        if (nums[pivot] < nums[high]) {
            high = pivot;
        } else if (nums[pivot] > nums[high]) {
            low = pivot + 1;
        } else {
            high -= 1;
        }
    }
    return nums[low];
}
```

 当允许重复值出现的时候，情况会变得复杂一些，比如上面的情况中如果mid等于right的话就没办法决定舍弃哪半部分了，最小值可能存在于任何一边，所以解决方法是当遇到这种情况有边界-1

