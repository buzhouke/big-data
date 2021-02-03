# 20201029N皇后

\[TOC\]

## 2020/10/29[N 皇后](https://leetcode-cn.com/problems/n-queens/)

本周是跟着B站教学视频学习回溯+剪枝。

### 八皇后问题

> **八皇后问题**（英文：**Eight queens**），是由国际西洋棋棋手马克斯·贝瑟尔于1848年提出的问题，是[回溯算法](https://baike.baidu.com/item/回溯算法/9258495)的典型案例。
>
> 问题表述为：在8×8格的国际象棋上摆放8个皇后，使其不能互相攻击，即任意两个皇后都不能处于同一行、同一列或同一斜线上，问有多少种摆法。

先放代码

```java
public class Queen {
    public static void main(String[] args) {
        new Queen().placeQueens(8);
    }
     int[] cols;
     int ways = 0;
     void placeQueens(int n){
        if(n < 1)
            return;
        cols = new int[n];
        place(0);
        System.out.println(n+"皇后一定有"+ways+"种摆法");
    }
    void place(int row){
         if(row == cols.length){
             ways++;
             show();
             return;
         }
      for(int col = 0; col<cols.length;col++){
          if(isVaild(row,col)){
                cols[row] = col;
                place(row+1);//执行不下去时则会回溯，重点在于对递归的一个理解
          }
      }
    }
    boolean isVaild(int row, int col){
         for(int i = 0;i < row;i++){//这里用了一个for循环，每次需要取出每一行的cols来对比是否已经放置皇后
             if(cols[i] == col)
                 return false;
             if(row -i == Math.abs(col-cols[i]))//这里涉及到了初中数学的k= 1 || -1来判定斜线
                 return false;

         }
         return true;
    }
    void show(){
         for(int row = 0; row < cols.length;row++){
             for(int col = 0; col < cols.length;col++){
                 if(cols[row] == col){
                     System.out.print("1  ");
                 }else {
                     System.out.print("0  ");
                 }
             }
             System.out.println();
         }
        System.out.println("---------------------------");
    }
}
```

先来简单讲讲对这个回溯的理解，其实就是对递归的一个应用，对于递归最容易理解的应该还是一颗二叉树，想象一下迷宫里，一个人走到岔路口，有两个位置可以走，走左边的人和走右边的人将产生两种不同的结果，并且不断地需要去做出选择，这一次是选择走左边还是走右边，在没有走到尽头（死胡同）时，这个人将一直走下去。

这里呢使用了cols数组来存放每一行皇后的摆放位置，再接着调用自身去计算下一行的皇后位置，如果在当前for循环中，isVaild（）函数的判断结果都是false，那么就会接着回溯上一行。

但不知道大家会不会有和我同样的想法呀，就是这个cols数组装的东西，不会被反复覆盖掉最后根本就没法正常实现记录功能嘛，打印出来才发觉自己糊涂了，明显也就是对于每一row，都将重新复制（覆盖掉），而这个递归的执行顺序时线性的，也就是选择走左边的人将走到尽头（死胡同）时，它将自己传送到上一次做出决策的地方，也就是上一个路口的另外一个方向。

![image-20201029211318916](https://i.loli.net/2020/10/29/G9IoEdVbaZy8ihq.png)

看下图应该对程序的执行顺序更为直观，重点关注place\(1\),place\(2\),place\(1\)这几个变动时发生了什么。

![image-20201029213745560](https://i.loli.net/2020/10/29/ckzx15jE2ZgPSGn.png)

![image-20201029214833782](https://i.loli.net/2020/10/29/yUo76OPihgwAzQs.png)

也就是在这个for循环中，在第一个if判断语句成立时，它进入下一个place（），当place（）走投无路，它会回到原本的for循环中其他成立的isValid（）下继续进行其他的递归。

### 优化一：对剪枝进行优化

查看原本的代码，发现isVaild（）的判断是用了一个for循环来进行，于是这一个块可以以空间换时间，节约至O（1）的复杂度。

但是效率虽然提高，但是会发现，我们没有办法去追踪这个八皇后是被摆在了什么位置，因为cols每次都被覆盖掉，解决方案是增加一个数组去存储（但这里就偷懒不写啦）。

```java
public class Queen2 {
    public static void main(String[] args) {
        new Queen2().placeQueens(8);
    }
     //int[] cols;
     int ways = 0;
     boolean[] cols;
     boolean[] leftTop;//左上角到右下角的斜线
     boolean[] rightTop;//右上角到左下角的斜线
     void placeQueens(int n){
        if(n < 1)
            return;
        cols = new boolean[n];
        leftTop = new boolean[(n<<1) -1];//2*n n左移一位
        rightTop = new boolean[leftTop.length];//此时已经不用再去做一次计算，尽量优化
        place(0);
        System.out.println(n+"皇后一定有"+ways+"种摆法");
    }
    void place(int row){
         if(row == cols.length){
             ways++;
             show();
             return;
         }
      for(int col = 0; col<cols.length;col++){
              if(cols[col])
                  continue;
              int ltIndex =row-col+cols.length-1;
              int rtIndex = row+col;
              if(leftTop[ltIndex])
                  continue;
              if(rightTop[rtIndex])
                  continue;
              cols[col] = true;
              leftTop[ltIndex] = true;
              rightTop[rtIndex] = true;
//                cols[row] = col;
                place(row+1);
          cols[col] = false;//还原代码
          leftTop[ltIndex] = false;
          rightTop[rtIndex] = false;

      }
    }
//    boolean isVaild(int row, int col){
////         for(int i = 0;i < row;i++){
////             if(cols[i] == col)
////                 return false;
////             if(row -i == Math.abs(col-cols[i]))
////                 return false;
////
////         }
//         return true;
//
//    }
    void show(){
//         for(int row = 0; row < cols.length;row++){
//             for(int col = 0; col < cols.length;col++){
//                 if(cols[row] == col){
//                     System.out.print("1  ");
//                 }else {
//                     System.out.print("0  ");
//                 }
//             }
//             System.out.println();
//         }
//        System.out.println("---------------------------");
    }
}
```

### 优化二：用位运算降低空间复杂度

这一点的思路在于布尔数组存的是true/fasle，true-》1，false-》0，在**八**皇后的情形下，一个数组可以替换成一个字节，如00100111 即`byte cols;`，两个字节则是使用`short rightTop`来替代

```java
public class Queen3 {
    public static void main(String[] args) {
        new Queen3().placeQueens();
    }
    int ways = 0;
    byte cols;
    short leftTop;//左上角到右下角的斜线
    short rightTop;//右上角到左下角的斜线

    void placeQueens() {
        place(0);
        System.out.println(8 + "皇后一定有" + ways + "种摆法");
    }
    void place(int row) {
        if (row == 8) {
            ways++;
            return;
        }
        for (int col = 0; col < 8; col++) {
            int cv = 1 << col;
            if ((cols & cv) != 0)
                continue;
            int ltIndex = 1 << (row - col + 7);
            int rtIndex = 1 << (row + col);
            if ((ltIndex & leftTop) != 0)
                continue;
            if ((rtIndex & rightTop) != 0)
                continue;
            //cols = (byte) (cols |(1<<col));
            cols |= (1 << col);
            leftTop |= ltIndex;
            rightTop |= rtIndex;
            place(row + 1);
            cols &= ~cv;
            leftTop &= ~ltIndex;
            rightTop &= ~rtIndex;
        }
    }
}
```

这里涉及的位运算技巧还是挺多的，我就先承认自己没掌握好啦

