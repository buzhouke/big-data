# 20200925Z字形变换

题目:[Z字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

先放上写得不漂亮侥幸通过的代码，

执行用时：79 ms, 在所有 Java 提交中击败了5.34%的用户，内存消耗：41.4 MB, 在所有 Java 提交中击败了5.01%

leetcode估计刷起来还是要点手感，长时间不刷就会做不出题来（太菜了，Java学得太烂了）

```java
class Solution {
    public String convert(String s, int numRows) {
        //模拟放入的过程，有几行，就设置几个容器（二维数组）去容纳，在容纳的过程中，也就是，对下标作出变换，实质用的还是charAt();
        if(numRows == 0 || s.length() ==0 ||numRows == 1)
        return s;
        int length = s.length();
        char[][] array  =new char[length][numRows];
        int k = 0;
        int i = 0;
        int j = 0;
        boolean flag = true;//true的时候，向下走，即j++；false的时候，向右上方走，i++，j--；
        array[0][0] = s.charAt(0);
        while(k < s.length()-1 &&  i <array.length && j < array[0].length){
            if(j == 0){
                j++;
                flag = true;
            }
            else if(j == array[0].length-1){
                flag = false;
                i++;
                j--;
            }
            else if(flag){
                j++;
            } 
            else if(!flag) {
                i++;
                j--;
            }             
            array[i][j] = s.charAt(k+1);
            k++;
        }
        String answer = "";
        for(int m = 0;m <array[0].length;m++){
            for(int n = 0;n <array.length;n++){          
                if(array[n][m]!='\0')
                answer += array[n][m];
            }
        }
        return answer;

    }
}
```

当然，看着官解又觉得自己悟了。

自然，再细看代码觉得自己又懵了。

```java
class Solution {
    public String convert(String s, int numRows) {

        if (numRows == 1) return s;

        List<StringBuilder> rows = new ArrayList<>();
        for (int i = 0; i < Math.min(numRows, s.length()); i++)
            rows.add(new StringBuilder());
        //使用min(numRows, s.length()) 个列表来表示 Z 字形图案中的非空行。
        int curRow = 0;
        boolean goingDown = false;

        for (char c : s.toCharArray()) {
            rows.get(curRow).append(c);
            if (curRow == 0 || curRow == numRows - 1) goingDown = !goingDown;
            curRow += goingDown ? 1 : -1;
        }

        StringBuilder ret = new StringBuilder();
        for (StringBuilder row : rows) ret.append(row);
        return ret.toString();
    }
}
```

先去扒了扒StringBuilder，记忆里就是个字符串拼接的东西，凭啥来作为泛型这样子用。

当然，作为一个追求源码与底层的菜鸡，先去翻完菜鸟教程再来假惺惺地对照源码学习：

**A mutable（可变的） sequence of characters.**  This class provides an API compatible with {@code StringBuffer}, but with **no guarantee of synchronization**.（线程不安全） This class is designed for use as a **drop-in** replacement（简单替换/即插即用） for {@code StringBuffer} in places where the string buffer was being used by a single thread \(as is generally the case\). Where possible, it is recommended that this class be used in preference to {@code StringBuffer} as it will be **faster** under most implementations. The principal operations on a {@code StringBuilder} are the {@code append} and {@code insert} methods, which are overloaded so as to accept data of any type. Each effectively converts a given datum to a string and then appends or inserts the characters of that string to the string builder. The {@code append} method always adds these characters at the end of the builder; the {@code insert} method adds the characters at a specified point.

For example, if {@code z} refers to a string builder object whose current contents are "{@code start}", then the method call {@code z.append\("le"\)} would cause the string builder to contain "{@code startle}", whereas {@code z.insert\(4, "le"\)} would alter the string builder to contain "{@code starlet}".

In general, if sb refers to an instance of a {@code StringBuilder}, then {@code sb.append\(x\)} has the same effect as {@code sb.insert\(sb.length\(\), x\)}.

Every string builder has a capacity. As long as the length of the character sequence contained in the string builder does not exceed the capacity, it is not necessary to allocate a new internal buffer. If the internal buffer overflows, it is automatically made larger.

Instances of {@code StringBuilder} are not safe for use by multiple threads. If such synchronization is required then it is recommended that {@link java.lang.StringBuffer} be used.

Unless otherwise noted, passing a {@code null} argument to a constructor or method in this class will cause a {@link NullPointerException} to be thrown.

@author Michael McCloskey @see java.lang.StringBuffer @see java.lang.String @since 1.5

认真读完之后大呼绝妙呀，每一行，都作为一个StringBuilder对象来搞数据，由于没有使用二维数组，因此就只用+1或-1来操作`rows.get(curRow)`，同时，也只用判断curRow来确定塞字母填充的方向。

到现在为止，每周三道leetcode的计划就算是完成了，很明显对比三四个月前，我降低了对独立思考的要求，而更多地要求自己从题解中学习新的知识，对照不足。

本周误打误撞，三篇都是关于字符串操作的，可以说，字符串操作其实门槛很低，主要还是侧重对于Java相关api的考察，而我本身的Java基础，确实是薄弱。（不止Java，其实code能力根本就不能看）

本周主要还是学习了charAt\(\)/StringBuilder/substring\(\)/copyValueOf\(，，\)等用法，就是回顾了一些之前很模糊的东西。

毕竟，现在是基本上确定走Java后端，夯实代码基础什么的，还是很重要。

