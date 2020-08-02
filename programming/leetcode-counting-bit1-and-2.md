# LeetCode:counting bit1&2

{% embed url="https://leetcode-cn.com/problems/single-number-ii/solution/zhi-chu-xian-yi-ci-de-shu-zi-ii-by-leetcode/" %}

发现自己对于位操作理解不是很好，找了两道题来练练手

两个题目分别要求找出一个数组中仅仅出现一次的元素（其他数字出现为基数次和偶数次）

## 其他数字出现为偶数次

```java
public int singleNumber(int[] nums) {
    int ans=0;
    for (Integer i : nums) {
        ans = ans^i;
    }
    return ans;
}
```

这里应用了XOR逻辑，异或逻辑就是两个相同出0不同出1，这里就会有应用，0和其他数XOR的结果就是这个数字（0和0出0，0和1出1）一个数字和自己异或的结果是0（0XOR0 = 0，1XOR1 =0），并且XOR可以交换运算，A XOR B XOR C = A XOR C XOR B。说到这里应该就清楚了，对于出现偶数次的数字，最终的XOR会得到0，对于出现奇数次的数字最终的XOR会是它自己，所以有了上面的代码，简洁清晰。

##  其他数字出现次数为奇数次

### 状态转移

```java
public int singleNumber2(int[] nums) {
    int seenOnce = 0, seenTwice = 0;

    for (int num : nums) {
        // first appearence:
        // add num to seen_once
        // don't add to seen_twice because of presence in seen_once

        // second appearance:
        // remove num from seen_once
        // add num to seen_twice

        // third appearance:
        // don't add to seen_once because of presence in seen_twice
        // remove num from seen_twice
        seenOnce = ~seenTwice & (seenOnce ^ num);
        seenTwice = ~seenOnce & (seenTwice ^ num);
    }

    return seenOnce;
}
```

看到这个解法的解释仿佛又回到了本科自动化的状态空间- -模电数电中的卡诺图状态转化逻辑- -着实怀念，这个老哥说的清楚[https://leetcode-cn.com/problems/single-number-ii/solution/luo-ji-dian-lu-jiao-du-xiang-xi-fen-xi-gai-ti-si-l/](https://leetcode-cn.com/problems/single-number-ii/solution/luo-ji-dian-lu-jiao-du-xiang-xi-fen-xi-gai-ti-si-l/)

### 倍数关系

```java
public int singleNumber3(int[] nums) {
    int[] counts = new int[32];
    for(int num : nums) {
        for(int j = 0; j < 32; j++) {
            counts[j] += num & 1;
            num >>>= 1;
        }
    }
    int res = 0, m = 3;
    for(int i = 0; i < 32; i++) {
        res <<= 1;
        res |= counts[31 - i] % m;
    }
    return res;
}
```

记录每一位上1出现的次数，最后每一位出现1的次数除以其他数字出现的次数（m）的余数（只可能是1或0），然后拼成一个32位的int数字就是答案

