# Java应用最广泛的类——String

想弄透一个东西还是从源码入手，先放上JDK8中的说明

### 源码说明

> The {@code String} class represents character strings. All string literals in Java programs, such as {@code "abc"}, are implemented as instances of this class.
>
> **Strings are constant;** their values cannot be changed after they are created. String buffers support mutable strings. Because String objects are immutable they can be shared. For example:
>
> > ```text
> >        String str = "abc";
> > ```
>
> is equivalent to:
>
> > ```text
> >        char data[] = {'a', 'b', 'c'};
> >        String str = new String(data);
> > ```
>
> Here are some more examples of how strings can be used:
>
> > ```text
> >        System.out.println("abc");
> >        String cde = "cde";
> >        System.out.println("abc" + cde);
> >        String c = "abc".substring(2,3);
> >        String d = cde.substring(1, 2);
> > ```
>
> The class {@code String} includes methods for examining individual characters of the sequence, for comparing strings, for searching strings, for extracting substrings, and for creating a copy of a string with all characters translated to uppercase or to lowercase. Case mapping is based on the Unicode Standard version specified by the {@link java.lang.Character Character} class.
>
> The Java language provides special support for the string concatenation operator \( + \), and for conversion of other objects to strings. String concatenation is implemented through the {@code StringBuilder}\(or {@code StringBuffer}\) class and its {@code append} method. String conversions are implemented through the method {@code toString}, defined by {@code Object} and inherited by all classes in Java. For additional information on string concatenation and conversion, see Gosling, Joy, and Steele, The Java Language Specification.
>
> Unless otherwise noted, passing a null argument to a constructor or method in this class will cause a {@link NullPointerException} to be thrown.
>
> A {@code String} represents a string in the UTF-16 format in which _supplementary characters_ are represented by _surrogate pairs_ \(see the section Unicode Character Representations in the {@code Character} class for more information\). Index values refer to {@code char} code units, so a supplementary character uses two positions in a {@code String}.
>
> The {@code String} class provides methods for dealing with Unicode code points \(i.e., characters\), in addition to those for dealing with Unicode code units \(i.e., {@code char} values\).

### 不可变性

这里很明确地写到，String对象是不可变的，并且提到了几个相关的类Character，StringBuilder。

结合《On Java 8》给出的代码来看，很明显这种“不可变”指的是该对象在调用一个方法后，仅返回值起变化，而String作为引用的对象是没有变的。这对于Java来说，有这么些好处：

1. 保证 String 对象的安全性。假设 String 对象是可变的，那么 String 对象将可能被恶意修改。 
2. 保证 **hash 属性值**不会频繁变更，确保了唯一性，使得类似 HashMap 容器才能实现相应的 key-value 缓存功能。
3.  可以实现**字符串常量池**。在 Java 中，通常有两种创建字符串对象的方式，一种是通过字符串常量的方式创建，如 String str=“abc”；另一种是字符串变量通过 new 形式的创建，如 String str = new String\(“abc”\)。

单单是讲清楚这个不可变性就值得另外再写一篇博客，这个是看起来写的比较简明易懂的：[https://www.cnblogs.com/dolphin0520/p/10693891.html](https://www.cnblogs.com/dolphin0520/p/10693891.html) ，额外针对这篇文章补充，对象与对象引用的区别，

```text
    String str = "abc"
    str = "abcd"
//str是String对象的引用，并非对象本身，对象在内存中是一块内存地址，
//str是一个指向该内存地址的引用，在两行代码执行后，“abc”对象仍然存在内存当中
```

  而对于字符串常量池，我们也可以通过代码来看是怎么一回事。

```text
        String string = new String("abc");
        String string2 = "abc";
        String string5 = "abc";
        System.out.println(string2 == string);//false
        System.out.println(string2 == string5);//true
        //也就是说，当我们使用string2的创建方式时，
        //JVM会首先检查该对象是否在字符串常量池中，
        //注意是用String重写的equals(Object obj)方法来检查，
        //只看字符串是否相同
        //如果有，返回的是对该对象的引用，否则就创建新的字符串
```

### 两种创建方式的区别

在使用String构造方法定义对象时，Java环境会和创建其他类型的对象一样，每次调用时，都会去创建一个新的对象，如上文中的string，我们还可以再创建个string7来验证下。

![&#x901A;&#x8FC7;new String\(&quot;&quot;\)&#x521B;&#x5EFA;&#x5BF9;&#x8C61;&#xFF0C;&#x6BCF;&#x6B21;&#x90FD;&#x662F;&#x65B0;&#x5BF9;&#x8C61;~~~](.gitbook/assets/image%20%283%29.png)

也就是这种方法并未检查字符串常量池。

### 字符串常量池

涉及JVM，先留坑。

![](.gitbook/assets/image%20%285%29.png)

### hash

### StringBuffer与StringBuilder，+

### ……



记录下知识盲区： 

* [ ] Java 关键字
* [ ] 函数式编程
* [ ] 反射



