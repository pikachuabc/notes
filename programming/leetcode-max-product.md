# LeetCode: max product

{% embed url="https://leetcode.com/problems/maximum-product-subarray/submissions/" %}

题目要求数组内连续的最大乘积，一看瞅过去好像和最大和的那个差不多，年轻了

```java
public int maxProductWrong(int[] nums) {
    if (nums.length == 1) {
        return nums[0];
    } else if (nums.length == 0) {
        return 0;
    }

    int[] dp = new int[nums.length];
    int maxPro = dp[0];
    dp[0] = nums[0];
    for (int i = 1; i < nums.length; i++) {
        dp[i] = Math.max(dp[i-1]*nums[i],nums[i]);
        if (dp[i] > maxPro) {
            maxPro = dp[i];
        }
    }
    return maxPro;
}
```

 行云流水的写下了以上DP，自以为无敌。

然后交了发现没有考虑负数的问题。。比如\[-2,1,-2\]，上面的解法会给出1，但是明显答案是4，于是思考了下，除了保存当前的最大值，我们还需要保存最小值以避免俩负数乘起来更大的情况

```java
public int maxProduct(int[] nums) {
    if (nums.length == 1) {
        return nums[0];
    } else if (nums.length == 0) {
        return 0;
    }

    int[][] dp = new int[nums.length][2];
    dp[0][0] = nums[0];
    dp[0][1] = nums[0];

    int maxPro = dp[0][0];

    for (int i = 1; i < nums.length; i++) {
        dp[i][0] = Math.max(Math.max(dp[i-1][0]*nums[i],dp[i-1][1]*nums[i]),nums[i]);
        dp[i][1] = Math.min(Math.min(dp[i-1][0]*nums[i],dp[i-1][1]*nums[i]),nums[i]);
        if (dp[i][0] > maxPro) {
            maxPro = dp[i][0];
        }
    }
    return maxPro;
}
```

 这里用了N\*2的dp数组，dp的方程很好想，dp\[i\]\[0\]保存第i个之前（包括i）的最大值，dp\[i\]\[1\]保存第i个前（包括i）的最小值，循环过程中除了比较当前数和前一个最大数字和当前数的乘积之外，还要比较和上一个最小数的乘积，以避免俩负数乘起来更大的情况

