# [二分查找专题练习](https://leetcode-cn.com/tag/binary-search/)

## 通用代码

```java
package Searches;

import java.util.Arrays;
import java.util.Random;
import java.util.concurrent.ThreadLocalRandom;
import java.util.stream.IntStream;

import static java.lang.String.format;

/**
 *
 *
 *
 * Binary search is one of the most popular algorithms
 * The algorithm finds the position of a target value within a sorted array
 *
 * Worst-case performance	O(log n)
 * Best-case performance	O(1)
 * Average performance	O(log n)
 * Worst-case space complexity	O(1)
 *
 *
 * @author Varun Upadhyay (https://github.com/varunu28)
 * @author Podshivalov Nikita (https://github.com/nikitap492)
 *
 * @see SearchAlgorithm
 * @see IterativeBinarySearch
 *
 */

class BinarySearch implements SearchAlgorithm {

    /**
     *
     * @param array is an array where the element should be found
     * @param key is an element which should be found
     * @param <T> is any comparable type
     * @return index of the element
     */
    @Override
    public  <T extends Comparable<T>> int find(T[] array, T key) {
        return search(array, key, 0, array.length);
    }

    /**
     * This method implements the Generic Binary Search
     *
     * @param array The array to make the binary search
     * @param key The number you are looking for
     * @param left The lower bound
     * @param right The  upper bound
     * @return the location of the key
     **/
    private <T extends Comparable<T>> int search(T array[], T key, int left, int right){
        if (right < left) return -1; // this means that the key not found

        // find median
        int median = (left + right) >>> 1;
        int comp = key.compareTo(array[median]);

        if (comp == 0) {
            return median;
        } else if (comp < 0) {
            return search(array, key, left, median - 1);
        } else {
            return search(array, key, median + 1, right);
        }
    }

    // Driver Program
    public static void main(String[] args) {
        // Just generate data
        Random r = ThreadLocalRandom.current();

        int size = 100;
        int maxElement = 100000;

        Integer[] integers = IntStream.generate(() -> r.nextInt(maxElement)).limit(size).sorted().boxed().toArray(Integer[]::new);


        // The element that should be found
        int shouldBeFound = integers[r.nextInt(size - 1)];

        BinarySearch search = new BinarySearch();
        int atIndex = search.find(integers, shouldBeFound);

        System.out.println(format(
            "Should be found: %d. Found %d at index %d. An array length %d",
            shouldBeFound, integers[atIndex], atIndex, size
        ));

        int toCheck = Arrays.binarySearch(integers, shouldBeFound);
        System.out.println(format("Found by system method at an index: %d. Is equal: %b", toCheck, toCheck == atIndex));
    }
}

```

## 题目：[面试题 10.05. 稀疏数组搜索](https://leetcode-cn.com/problems/sparse-array-search-lcci/)

知识点一：二分查找

知识点二：Java API，按字典序对比，

| Modifier and Type | Method and Description |
| :---------------- | :--------------------- |
| `int` | `compareTo(String anotherString)`Compares two strings lexicographically. |

知识点三：稀疏字符串数组，需考虑空字符串

### 可参考题解

```java
class Solution {
    public int findString(String[] words, String s) {
      int left = 0;
      int right = words.length-1;
      return find(words,left,right,s);
    
    }
     int find(String[] words,int left,int right,String s){
      while(left<right && words[left].equals("")) // 1.左右缩界，直到找到非空String
      {
            left++;
      }
     
      while(left<right && words[right].equals(""))
      {
            right--;
      }
      if(s.compareTo(words[right]) > 0 || s.compareTo(words[left]) < 0) return -1; 
      //1.1若左边界的值都大于String，说明数组中不存在此s
      //1.2同理，若右边界的值都小于String，说明数组中不存在此s

      int mid = left + (right - left)/2;//2.寻找中间节点，促使二分法进行
    
      while(mid > left && words[mid].equals("")) //2.1防止中间节点为空，向左找一直找到非空的word【mid】
      {
            mid--;
      }
       if(words[mid].equals(s)) return mid; //2.2若与s相同，则说明找到了解，返回mid
     
      int co = s.compareTo(words[mid]);
    //按字典序排序

      if(co<0) //3 根据比较结果决定向左或向右查找
      return find(words,left,mid-1,s); 
      if(co>0)
      return find(words,mid+1,right,s);
       
       return -1;//左右都没有返回值，说明无解，返回
     }

}
```

