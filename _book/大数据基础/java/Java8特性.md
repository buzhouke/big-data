# Java 8特性

函数式编程

## 接口默认方法

接口加上关键字**default**可以定义具体的方法：

```java
package site.buzhou.java8;

public interface Animal {
    default void IntroduceSelf(String name){
        System.out.println("My name is "+name);
    }
}

```

```java
package site.buzhou.java8;

/**
 * @program: OnJava8
 * @description:
 * @author: 不周
 * @create: 2021-02-21 12:09
 **/
public class Cat implements Animal {
}

```

```java
package site.buzhou.java8;

/**
 * @program: OnJava8
 * @description:
 * @author: 不周
 * @create: 2021-02-21 12:09
 **/
public class Test {
    public static void main(String[] args) {
        Cat cat = new Cat();
        cat.IntroduceSelf("Vivien");
    }
}

```

![image-20210221121848006](https://buhzou-1300695412.cos.ap-nanjing.myqcloud.com/image-20210221121848006.png)

## Lamda表达式

接着上面的继续写，我们常常以内部类的方式来重写方法：

在Animal接口中增加一个talkSomething的方法，在Test代码中我们试着重写接口方法，但发现上文讲的默认方法编译器还是自动让我们重写了：

```java
Animal animal = new Animal() {
            @Override
            public void IntroduceSelf(String name) {
                System.out.println("Your name is "+name);
            }

            @Override
            public void talkSomething(String something) {
				System.out.println(something);
            }
        };
animal.IntroduceSelf("Vivian");
//print：Your name is Vivian
```

![image-20210221124457733](https://buhzou-1300695412.cos.ap-nanjing.myqcloud.com/image-20210221124457733.png)

这种内部类的方式，也就意味着代码无法重用，唯一的目的就是去调用我们新写的方法，`animal.IntroduceSelf("Vivian");`,`animal.talkSomething("I don't want to talk anything");`。在Java8中，提供了更快捷的方式让我们去完成这一目的。

```java
 Animal testAnimal = (something) ->{
            System.out.println(something);
        };
        //发现重写的是未加default的方法。
        testAnimal.IntroduceSelf("King");
        //print: My name is King
        testAnimal.talkSomething("King");
		//print: King
```

我们用`(something)` 替换了下面这一长串，

```java
new Animal() {
            @Override
            public void IntroduceSelf(String name) {
                System.out.println("Your name is "+name);
            }

      		@Override
        	public void talkSomething(String something)
```

同时，我们有更简略的写法：

```java
 Animal testAnimal = System.out::println;
```

接下来，我们试试，如果一个接口中定义两个非默认方法，发现报错了，

![image-20210221134013194](https://buhzou-1300695412.cos.ap-nanjing.myqcloud.com/image-20210221134013194.png)

编译器提示不是一个函数式接口，也就是，函数式接口只能定义一个非默认方法，Java8也给我们提供了一个注解来标识函数式接口@FunctionalInterface

![image-20210221134236702](https://buhzou-1300695412.cos.ap-nanjing.myqcloud.com/image-20210221134236702.png)

其作用域类似于匿名内部类



……

留坑待填