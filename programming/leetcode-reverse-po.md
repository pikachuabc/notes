# LeetCode: reverse-polish-notation

[https://leetcode.com/problems/evaluate-reverse-polish-notation/](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

一看见这个题就心痛....算法考试在没有任何提示的情况下要写一个计算器的实现。。是我太菜了

```java
public int evalRPN(String[] tokens) {
    Stack<Integer> calculate = new Stack<>();
    for (String token : tokens) {
        int temp1;
        int temp2;
        switch (token) {
            case "+":
                temp1 = calculate.pop();
                temp2 = calculate.pop();
                calculate.push(temp1 + temp2);
                break;
            case "-":
                temp1 = calculate.pop();
                temp2 = calculate.pop();
                calculate.push(temp2 - temp1);
                break;
            case "*":
                temp1 = calculate.pop();
                temp2 = calculate.pop();
                calculate.push(temp1 * temp2);
                break;
            case "/":
                temp1 = calculate.pop();
                temp2 = calculate.pop();
                calculate.push(temp2 / temp1);
                break;
            default:
                calculate.push(Integer.parseInt(token));
        }
    }
    return calculate.pop();
}
```

 主要是栈的应用ez



