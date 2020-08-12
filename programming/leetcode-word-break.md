# LeetCode:word-break

{% embed url="https://leetcode-cn.com/problems/word-break/solution/dan-ci-chai-fen-by-leetcode-solution/" %}

另一道没做出来的DP= =道行还是太浅

```java
public boolean wordBreak(String s, List<String> wordDict) {
    Set<String> wordDictSet = new HashSet(wordDict);
    boolean[] dp = new boolean[s.length() + 1];
    dp[0] = true;
    for (int i = 1; i <= s.length(); i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && wordDictSet.contains(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[s.length()];
}
```

来慢慢分析这个DP，先上DP的状态转移公式

$$
dp[i] = dp[j] AND check(s[j..i−1])
$$

dp是状态列表，长度为s.length+1，dp\[i\]的含义是对于字符串s中的前i个字符是否满足条件（可以由字典中的字符串组成）而对于前i字符，又可以被j分成两部分，如果这两部分均是满足条件的字符串，那么前i个字符也满足条件，这两部分一部分是\[0:j\)，另一部分是\[j:i\)，不难发现\[0:j\)可以通过dp\[j\]判断是否满足条件（对于s的角标来说是j-1，但是dp数组的0角标用作头了所以是j），所以只需要判断\[j:i\)这一部分就可以最后返回dp\[s.length\(\)\]就可以知道这个字符串是否满足条件

