
#### 1.简要概述 面向对象的特征，谈谈自己的理解
#### 2. 输出结果
```java
        Integer i1 = 100;
        Integer i2 = 100;
        Integer i3 = 1000;
        Integer i4 = 1000;
        String s1 = "androidLab";
        String s2 = "androidLab";
        String s3 = new String("androidLab");
        String s4 = new String("androidLab");
        String s5 = "android";
        String s6 = "Lab";
        String s7 = new String("android");
        String s8 = new String("Lab");
        String s9 = s5 + s6;
        String s10 = new String("androidLab").intern();
        
        System.out.println( i1 == i2 );
        System.out.println( i3 == i4);
        System.out.println( s1 == s2 );
        System.out.println( !s1.equals(s2) );
        System.out.println( s1 == s3 );
        System.out.println( s3 == s4 );
        System.out.println( s1 == ( s5 + s6 ) );
        System.out.println( s1 == s9 );
        System.out.println( s3 == ( s5 + s8) );
        System.out.println( s3 == s10 );
        
```
#### 3.输出结果
![](http://onf44qqgp.bkt.clouddn.com/17-11-3/95365744.jpg)
#### 4.写出至少一种自己熟悉的 java 设计模式
#### 5.利用java代码分析回调机制
#### 6.内存溢出（OutOfMemoryError）和内存泄漏的区别，试着分析（可以利用 java代码，或者Android具体情形）

#### 7.至少完成一个(任意一种计算机语言【兄弟别用 opencv 】)
* 给定一个图片路径（d://test.jpg）利用代码将它复制到（d://test1.jpg）
* 遍历D盘所有文件输出以（.txt）结尾的文件名称
#### 8.ArrayList 和 LinkedList 的区别，对可变参数的理解(可以写出代码)
#### 9 写下在实验室一年你学到的东西，以及今后的打算（语言不限 eg：English）
