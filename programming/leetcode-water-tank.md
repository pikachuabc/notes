# LeetCode:water tank

{% embed url="https://leetcode.com/problems/container-with-most-water/" %}

## Brute force

```java
    public int maxArea(int[] height) {
        int maxArea = 0;
        int area = 0;
        for (int i = 0; i < height.length-1; i++) {
            for (int j = i+1; j < height.length; j++) {
                area = Math.min(height[i],height[j])*(j-i);
                maxArea = Math.max(area, maxArea);
            }
        }
        return maxArea;

    }
```

遍历所有可能情况，计算面积～～easy 但是



{% page-ref page="leetcode-water-tank.md" %}



