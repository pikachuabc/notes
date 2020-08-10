# Leetcode:climbStair

连接丢了= =不过题目比较通俗易懂，给定n个台阶一次可以上1-2个台阶，有多少种可能性，一眼看见这不是蜗牛趴井的问题（不过这个题不能超出去）= =，DP或者递归可解

## 递归

```java
public int climbStairs(int n) {
    if (n == 2) {
        return 2;
    } else if (n == 1) {
        return 1;
    } else if (n == 0) {
        return 0;
    }
    return climbStairs(n - 1) + climbStairs(n - 2);
}
```

思路：如果要到当前的台阶，那么可以从n-1和n-2级台阶上来，0，1，2属于递归的终点

$$
f(n) = f(n-1)+f(n-2)
$$

## DP

```java
public int climbStairs1(int n) {
    if (n == 0) {
        return 0;
    }

    int[] a = new int[]{1, 2};
    int index = 0;
    for (int i = 3; i <= n; i++) {
        a[index] = a[0] + a[1];
        index = index == 0 ? 1 : 0;

    }
    return (n&1)==1? a[0]:a[1];
}
```

DP的思路其实和递归是一样的，不过这里可以节省很多空间相当于走n步，汉诺塔问题，不用储存中间状态，所以只需要2个空间大小的数组就可以，结果取决于n的奇偶性，返回a\[0\]或者a\[1\]

```java
 return (n&1)==1? a[0]:a[1];
```



