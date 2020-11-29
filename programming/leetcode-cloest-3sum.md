# LeetCode: cloest 3sum

{% embed url="https://leetcode-cn.com/problems/3sum-closest/" %}

求一个数组中三个数字之和，要求于target的距离最小

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int ans = nums[0] + nums[1] + nums[2];
        for(int i=0;i<nums.length;i++) {
            int start = i+1, end = nums.length - 1;
            while(start < end) {
                int sum = nums[start] + nums[end] + nums[i];
                if(Math.abs(target - sum) < Math.abs(target - ans))
                    ans = sum;
                if(sum > target)
                    end--;
                else if(sum < target)
                    start++;
                else
                    return ans;
            }
        }
        return ans;
    }
}
```

排序加双指针的解法，暴力遍历的话到n3。排序nlogn，外面的嵌套加双指针n方，最后n方

