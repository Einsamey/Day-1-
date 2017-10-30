
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
        
        System.out.println( i1 == i2 );
        System.out.println( i3 == i4);
        System.out.println( !s1.equals(s2) );
        System.out.println( s1 == s2 );
        System.out.println( !s1.equals(s2) );
        System.out.println( s1 == s3 );
        System.out.println( s3 == s4 );
        System.out.println( s1 == ( s5 + s6 ) );
        System.out.println( s1 == s9 );
        System.out.println( s3 == ( s5 + s8) );
```

#### 3.试着分析( 文字加代码 )至少一种自己熟悉的 java 设计模式
#### 4.写出对 static final 的理解
#### 5.将以下代码改成依赖注入的形式(提示: 构造方法注入，Setter方法注入，接口注入，注解注入  【任意选择一种即可】)，
```java
public class Team {
    People people;

    public Team() {
        this.people = new People("android",5);
    }
}

```
#### 6.至少完成一个
* 给定一个图片路径（d://test.jpg）利用代码将它复制到（d://test1.jpg）
* 遍历D盘所有文件输出以（.txt）结尾的文件名称
#### 7.写下在实验室一年你学到的东西，以及今后的打算

