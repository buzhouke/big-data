# 20200922最长回文子串

阅读预警：！仅为个人学习总结，知识性针对性较弱！

看回leetcode感觉自己确实是十分菜，相当菜，十分钟想思路，20分钟找相应的方法然后磕磕绊绊地写代码，再来十分钟对着leetcode的不友好报错debug，再是看着自己的代码感觉再改下去浪费时间，再花十分钟看题解

:arrow\_up:菜鸡的刷题之路

这道题，对于题目本身，最直观的想法肯定是将回文串，对叠，亦即题解中说的“中心扩散法”，

## 中心扩散法

回顾自己写的垃圾代码，最为致命的一点是，在不了解有哪些东西可用的情况下，点开菜鸟教程，找到了记忆里的CobyValueOf（）

```java
public class Test {
    public static void main(String args[]) {
        char[] Str1 = {'h', 'e', 'l', 'l', 'o', ' ', 'r', 'u', 'n', 'o', 'o', 'b'};
        String Str2 = "";

        Str2 = Str2.copyValueOf( Str1 );
        System.out.println("返回结果：" + Str2);

        Str2 = Str2.copyValueOf( Str1, 2, 6 );
        System.out.println("返回结果：" + Str2);
    }
}
```

开始了将字符串转char\[\]的过程，繁琐且易出错，而忽视了String本身的charAt（），另外在搞类似“双指针”的时候，总是学不乖用for，然后目瞪口呆地看着leetcode破编译器报错，是的，它竟然不支持这种写法，

```java
 for (int i = 0, j = str.length() - 1; i < j; i++, j--) {
            char temp = value[i];
            value[i] = value[j];
            value[j] = temp;
        }
```

题解的写法就是好看哭唧唧，

```text
public class Solution {

    public String longestPalindrome(String s) {
        int len = s.length();
        if (len < 2) {
            return s;
        }
        int maxLen = 1;
        String res = s.substring(0, 1);
        // 中心位置枚举到 len - 2 即可
        for (int i = 0; i < len - 1; i++) {
            String oddStr = centerSpread(s, i, i);
            String evenStr = centerSpread(s, i, i + 1);
            String maxLenStr = oddStr.length() > evenStr.length() ? oddStr : evenStr;
            if (maxLenStr.length() > maxLen) {
                maxLen = maxLenStr.length();
                res = maxLenStr;
            }
        }
        return res;
    }

    private String centerSpread(String s, int left, int right) {
        // left = right 的时候，此时回文中心是一个字符，回文串的长度是奇数
        // right = left + 1 的时候，此时回文中心是一个空隙，回文串的长度是偶数
        int len = s.length();
        int i = left;
        int j = right;
        while (i >= 0 && j < len) {
            if (s.charAt(i) == s.charAt(j)) {
                i--;
                j++;
            } else {
                break;
            }
        }
        // 这里要小心，跳出 while 循环时，恰好满足 s.charAt(i) != s.charAt(j)，因此不能取 i，不能取 j
        return s.substring(i + 1, j);
    }
}
```

接下来，简单总结（学习）他人题解的优点：

1、 `String res = s.substring(0, 1);`，一个res走天下系列

2、用while来写循环（妈耶大三了咋连点优化写法都不知道）

3、考虑奇数偶数的各异情况（而我却、、就是看着自己的写法搞这个搞不太来因此放弃进一步debug，放任代码废掉）

4、针对原有的字符串进行操作而非另外弄个数组

摸索着基本领会了上述代码的写法，效率果然比自己死抠代码高了那么一丢丢，值得庆祝。

## 动态规划

针对下一种解法，动态规划，“对于一个子串而言，如果它是回文串，并且长度大于 22，那么将它首尾的两个字母去除之后，它仍然是个回文串。例如对于字符串 ”ababa''，如果我们已经知道 “bab” 是回文串，那么“ababa” 一定是回文串，这是因为它的首尾两个字母都是 “a”“，从这个角度来思考，翻回之前所学动态规划时概括的，”dp：**能将大问题拆成几个小问题，且满足无后效性、最优子结构性质。**“，也是能去拆分问题。

这里可以体会到，这种拆分，将大问题拆解为小问题，是计算机领域相当常见的思维，深挖到最后也就是0&1；

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        String ans = "";
        for (int l = 0; l < n; ++l) {
            for (int i = 0; i + l < n; ++i) {
                int j = i + l;
                if (l == 0) {
                    dp[i][j] = true;
                } else if (l == 1) {
                    dp[i][j] = (s.charAt(i) == s.charAt(j));
                } else {
                    dp[i][j] = (s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1]);
                }
                if (dp[i][j] && l + 1 > ans.length()) {
                    ans = s.substring(i, i + l + 1);
                }
            }
        }
        return ans;
    }
}
```

