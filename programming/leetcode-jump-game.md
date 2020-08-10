# LeetCode:jump game

{% embed url="https://leetcode-cn.com/problems/jump-game/submissions/" %}

## 自己的思路

```java
public boolean canJump(int[] nums) {

    for (int i = nums.length - 1; i >= 0; i--) {
        if (nums[i] == 0 && i != nums.length - 1) {
            int temp = 0;
            for (int j = i; j >= 0; j--) {
                if (nums[j] <= temp) {
                    temp++;
                } else {
                    break;
                }

                if (j == 0) {
                    return false;
                }
            }

        }
    }
    return true;
}
```

这个问题思考后可以得出，一旦走到0就GG，所以自己的思路就是从数组的末尾找0，找到之后，对于0之前的数字，只要存在自己的数字比距离0的距离大，那么这个0就可以跳过去，safe，只要有一个0无法避免，就返回false

## 大佬的思路

```java
public boolean canJump1(int[] nums) {
    int n = 1;
    for (int i = nums.length - 2; i >= 0; i--) {
        if (nums[i] >= n) {
            n = 1;
        } else {
            n++;
        }
        if (i == 0 && n > 1) {
            return false;
        }
    }
    return true;

}
```

大佬的思路则是每次只检查数组的末尾然后切割数组，只要可达就切掉从可达点之后的部分，这样算法可以达到O\(n\)的时间复杂度

