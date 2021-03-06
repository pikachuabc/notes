# LeetCode: decode string

{% embed url="https://leetcode-cn.com/problems/decode-string/" %}

这个题还蛮有意思的，按照要求的方式去解析一个字符串

```java
public String decodeString(String s) {

    Stack<String> stackA = new Stack<>();
    Stack<String> stackB = new Stack<>();
    for (int i = 0; i < s.length(); i++) {
        int j = i+1;
        while (Character.isDigit(s.charAt(i))&&j<s.length() && Character.isDigit(s.charAt(j))) {
            j++;
        }
        stackA.push(s.substring(i,j));
        i=j-1;
    }

    while (!stackA.isEmpty()) {
        String temp1;
        while (!stackA.isEmpty() && !(temp1 = stackA.pop()).equals("[")) {
            stackB.push(temp1);
        }
        if (stackA.isEmpty()) {
            continue;
        }

        String temp2;
        StringBuilder temp3 = new StringBuilder();
        while (!(temp2 = stackB.pop()).equals("]")) {
            temp3.append(temp2);
        }
        StringBuilder temp4 = new StringBuilder();
        int repeatTime = Integer.parseInt(stackA.pop());
        for (int i = 0; i < repeatTime; i++) {
            temp4.append(temp3);
        }
        stackA.push(temp4.toString());
    }
    StringBuilder result = new StringBuilder();
    while (!stackB.isEmpty()) {
        result.append(stackB.pop());
    }
    return result.toString();
}
```

自己写的有点臃肿但是速度和空间都是70+，主要思路是通过两个stack对字符串进行解析 

```java
for (int i = 0; i < s.length(); i++) {
        int j = i+1;
        while (Character.isDigit(s.charAt(i))&&j<s.length() && Character.isDigit(s.charAt(j))) {
            j++;
        }
        stackA.push(s.substring(i,j));
        i=j-1;
    }
```

这部分是将输入字符串压入栈A中，中间那个while判断逻辑是为了将数字（任意位数）作为一个整体压入string的stack中。

再下面就是逻辑的主要部分，不断把stackA中的字符串pop然后push到stackB中，直到pop出来一个"\["，这个时候就去stackB pop直到遇到一个"\]"，中间的部分用stackA pop出来的数字作为重复次数，**生成结果后再压入A中，直到A变成空的（没有需要解析的部分了），这时候stackB中依次pop出来就是解析后的字符串**

