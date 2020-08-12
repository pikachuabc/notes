# LeetCode:longest-increasing-subsequence

{% embed url="https://leetcode-cn.com/problems/longest-increasing-subsequence/" %}

经典DP问题，没有做出来= =给自己一个嘴巴子

```java
public int lengthOfLIS(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    int[] dp = new int[nums.length];
    dp[0] = 1;
    int maxans = 1;
    for (int i = 1; i < dp.length; i++) {
        int maxval = 0;
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                maxval = Math.max(maxval, dp[j]);
            }
        }
        dp[i] = maxval + 1;
        maxans = Math.max(maxans, dp[i]);
    }
    return maxans;
}
```

DP的问题主要是想清楚状态转移的方程这里是每个点的最长序列，如下所示，其中dp\[0\]....dp\[j\]指的是nums\[0\]...nums\[i\]作为结尾数字的最长序列数

$$
dp[i]=max(dp[0]....dp[j])+1
$$

算法中for循环哪的maxval指的是对于当前的nums\[i\]，选择所有前面之中拥有最大序列的数字的序列长度，加1后就是i这个数字最大序列长度，挺巧妙的

