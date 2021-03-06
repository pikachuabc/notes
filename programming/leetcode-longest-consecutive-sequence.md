# LeetCode: longest-consecutive-sequence

{% embed url="https://leetcode-cn.com/problems/longest-consecutive-sequence/" %}

这个题要求求一个数组中的最长序列，主要是要求时间复杂度O\(n\)

第一眼看到这个题很容易是想到排序，然后遍历数组寻找最长的序列

```java
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    //long startTime = System.currentTimeMillis();
    Arrays.sort(nums);
    //long endTime = System.currentTimeMillis();
    //System.out.println("sort time= " + (endTime - startTime));
    int ans = 1;
    int max = 1;
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] == nums[i - 1] + 1) {
            ans++;
            if (max < ans) {
                max = ans;
            }
        }
        if (nums[i] != nums[i - 1] && nums[i] != nums[i - 1] + 1) {

            ans = 1;
        }
    }
    return max;
}
```

 答案给的是hashset实现，先用set去重，然后对于每一个数字，在set中用contain查看是否有前驱数字，这个还是挺巧妙的，因为如果一个数字有前驱的数字那么最长的序列的开头一定不是它，所以也就不用往后寻找序列了。所以只需要遍历这个数组就可以得到答案On解法

```java
public int longestConsecutive1(int[] nums) {

    Set<Integer> num_set = new HashSet<Integer>();
    //long startTime = System.currentTimeMillis();
    for (int num : nums) {
        num_set.add(num);
    }
    //long endTime = System.currentTimeMillis();
    //System.out.println("input time= " + (endTime - startTime));
    int longestStreak = 0;

    for (int num : num_set) {
        if (!num_set.contains(num - 1)) {
            int currentNum = num;
            int currentStreak = 1;

            while (num_set.contains(currentNum + 1)) {
                currentNum += 1;
                currentStreak += 1;
            }

            longestStreak = Math.max(longestStreak, currentStreak);
        }
    }

    return longestStreak;
}
```

 有意的是用arraysort方法提交后暴打99%，其实是一个数据量的问题，但是之后测试后又觉得挺有意思，sort方法在数据量大的时候表现的很不稳定，好的时候比hashset少3分之一的时间，坏的时候也只多一点，其实还是arrays.sort这个方法，对于sort的解法，之后遍历数组的消耗是比hashset用contain方法快一些，而sort方法对于almost sorted以及数组体积比较小的排序做了很多优化，所以导致大部分测试例子下sort方法会快很多，对于arrays.sort分析可以看之前的分析

{% page-ref page="leetcode-sort-list.md" %}



