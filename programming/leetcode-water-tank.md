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

遍历所有可能情况，计算面积～～easy 但是时间复杂度到n方了

## 双指针

```java
public int maxArea(int[] height) {
    int i = 0;
    int j = height.length-1;
    int maxArea = 0;
    while (i<j){
        maxArea = Math.max(maxArea,Math.min(height[i],height[j])*(j-i));
        if (height[i]>height[j]){
            j--;
        }else if (height[i]<height[j]){
            i++;
        }else {
            //In this case you can move both pointers at once.
            // If you moved one pointer, because they're the same height,
            // the height of the next area will become smaller. T
            // he width will become smaller, hence the area will be smaller.
            i++;
            j--;
        }
    }
    return maxArea;
}
```

两指针分别从左右向中间靠近，因为j-i是减小的，所以面积大小实际上取决于高度较低的那个，所以每次移动较低的那个就可以

