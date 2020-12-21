# LeetCode:subsets

{% embed url="https://leetcode.com/problems/combination-sum/" %}

求一个数组的所有子集，包括空集

```java
public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        result.add(new ArrayList<>());
        for (int num : nums) {
            int timer = result.size();
            for (int i1 = 0; i1 < timer; i1++) {
                ArrayList<Integer> temp = new ArrayList<>(result.get(i1));
                temp.add(num);
                result.add(temp);
            }
        }
        return result;
    }
```

以前做过一次有点忘了重做一下。首先加一个空元素进result，对于每一个nums中的数字。将其添加到现存的每一个list中变成一个新的list添加到result中，即每次result的size翻倍

