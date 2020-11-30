# LeetCode: first and last position in sorted array

{% embed url="https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/" %}

求一个排序数组中target值的开始和结束位置，重点在于要求复杂度为logn

不考虑复杂度的话直接遍历数组找到开始结束位置即可，logn的复杂度考虑用二分查找的方法，如下，二分查找的模版，这里注意判断条件是startIndex &lt;= endIndex，不加等号的话会造成无法找到最后一个元素 （mid+1后如果在最后一位会导致midindex=startindex从而从while中出来）

```java
public static int binSearch(int[] nums, int startIndex, int endIndex, int target) {
        while (startIndex <= endIndex) {
            int midIndex = (startIndex + endIndex) / 2;
            if (nums[midIndex] > target) {
                endIndex = midIndex-1;
            } else if (nums[midIndex] < target) {
                startIndex = midIndex+1;
            } else {
                return midIndex;
            }
        }
        return -1;
    }
```

开始之后开始解题

```java
public int[] searchRange1(int[] nums, int target) {
        int startIndex = 0;
        int endIndex = nums.length-1;
        int hit = binSearch(nums,startIndex,endIndex,target);
        if (hit==-1)return new int[]{-1,-1};
        if ((hit == 0 || nums[hit - 1] != target) && (hit == nums.length-1 || nums[hit + 1] != target)) {
            return new int[]{hit,hit};
        }
        int leftBound = hit;
        //find left bound
        while ( leftBound!=0 && nums[leftBound - 1] == target) {
            leftBound = binSearch(nums,0,leftBound-1,target);
        }
        int rightBound = hit;
        while ( rightBound!=nums.length-1 &&  nums[rightBound + 1] == target) {
            rightBound = binSearch(nums,rightBound+1,nums.length-1,target);
        }
        return new int[]{leftBound,rightBound};
    }
```

二分查找找到目标，以这个index分割数组分别找左右bound，比如找左bound的条件就是对于找到的index判断左边的数字是否仍然等于自己，是的话就继续从左边的数组找，右侧同理，但是不太确定这样算logn还是nlogn = =

