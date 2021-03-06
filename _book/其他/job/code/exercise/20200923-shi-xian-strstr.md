# 20200923实现 StrStr\(\)

题目: [实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

这道题目，乍看上去还是挺简单的，于是一边听着学妹讲话一边不过脑子的写代码，事实证明，即便经过了昨天最长回文子串的历练，我仍然不够熟悉字符串的题目，

先放上自己卡用例改半天都过不了的代码。

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length()== 0)
            return 0;
        int i = 0;
        int j = 0;
        boolean flag = false;
        while(i < haystack.length() && j < needle.length()){
            if(haystack.charAt(i) == needle.charAt(j)){
                i++;
                j++;
                flag = true;              
            }
            else{
                i++;
                flag = false;
            }
        }
        if(flag && haystack.length() >= needle.length()){
            return i - j;
        }
        else
        return -1;
    }
}
```

很明显，对照官方题解，是在以滑动窗口的思想在解题目，但是，却忽视了一个根本问题，”**字符串匹配因根据子串来匹配而非单个字符一个个比对**“，以至于代码最后需重写。

官解：

```java
class Solution {
  public int strStr(String haystack, String needle) {
    int L = needle.length(), n = haystack.length();

    for (int start = 0; start < n - L + 1; ++start) {
      if (haystack.substring(start, start + L).equals(needle)) {
        return start;
      }
    }
    return -1;
  }
}
```

于是，回顾上一篇总结，这是根本没去理解算法/代码本身，得出了一些很肤浅的结论又迫不及待地加以利用。

但是这里还是要给阅读后的结论

1. `++start`这个写法室友跟我讲过，是能够卡时间复杂度还是什么，能编译快一点
2. 由于是拿子串对比，将for循环限制在`start < n - L + 1`是十分必要也是需要我去学习的，这是去一段一段地截取出haystack，然后与needle对比。
3. 应该注意用equals而非==。

