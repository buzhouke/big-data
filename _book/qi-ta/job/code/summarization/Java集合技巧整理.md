## 链接整理

[一个博主整理的Java集合，虽然是JDK1.6但是很详细很Nice了: Java集合目录](https://www.cnblogs.com/skywang12345/p/3323085.html)

[集合与数组互转](https://blog.csdn.net/huanghanqian/article/details/73920439)

## 集合使用技巧

### Collection

List集合转int()数组,利用 jdk8 流特性：

```java
ArrayList<Integer> list = new ArrayList<>();
list.stream().mapToInt(Integer::intValue).toArray()
```

### Map

用了都说好的HashMap统计出现频率

```java
 HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
 map.put(i,map.getOrDefault(i, 0)+1)
```

