---
title: Java学习笔记07
date: 2020-01-04 22:00:57
tags:
	- Java
	- 基础
	- 常见对象
	- Scanner
	- String
	- StringBuffer
	- StringBuilder
	- Arrays
	- Integer
	- 基本类型包装类
categories: Java学习笔记
---

### 常见对象（API）

* Java SE 13 & JDK 13 API 文档

[Java® Platform, Standard Edition & Java Development Kit Version 13 API Specification](https://docs.oracle.com/en/java/javase/13/docs/api/index.html)

<!--more-->

#### Scanner

* Scanner是java.util包下的，所以在使用时要导包`import java.util.Scanner;`

```java
	Scanner sc = new Scanner(System.in);
	int i = sc.nextInt();
```

* 常见方法

  * hasNextXxx: 判断是否还有下一个输入j项，Xxx可以是Int，Double等，返回boolean的值

  * nextXxx: 获取下一个输入项，默认使用空格，回车等作为分隔符

    * nextInt: 获取Int类型的值
    * nextLine: 获取一个String类型的值

    ```java
    int i = sc.nextInt();
    String line = sc.nextLine();
    ```

    * 此时会出现一个问题：nextInt()是键盘录入整数，只接收整数；而nextLine是接收的是字符串，以\r\n为结束。所以在录入数字后回车，i接收了整数，而line以\r\n结束，无法继续输入。
    * 解决办法：①创建两个Scanner对象；②都以next Line接收

#### String

* String是java.lang包下的，使用时不需要导包

* 由final修饰，没有子类

* String对象是不可变的，字符串被储存在常量池中，可以共享

* String类的构造方法：
  * ` public String() `：空构造
  * ` public String(byte[] bytes) `：字节数组转成字符串
  * ` public String(byte[] bytes,int index,int length) `：字节数组中的一部分转成字符串
  * ` public String(char[] value) `：字符数组转成字符串
  * ` public String(char[] value,int index,int count) `：字符数组中的一部分转成字符串
  * ` public String(String original) `：字符常量转成字符串
* String类的判断方法
  * ` boolean equals(object obj) `： 比较字符串是否相同，区分大小写
  * ` boolean equalsIgnoreCase(String str) `：比较字符串是否相同，不区分大小写
  * ` boolean contains(String str) `：大字符串是否包括小字符串
  * ` boolean startWith(String str) `：判断字符串是否以某个指定的字符串开始
  * ` boolean endWith(String str) `：判断字符串是否以某个指定的字符串结束
  * ` boolean isEmpty() `：判断字符串是否为空

* String类的获取方法
  * ` int length() `：获取字符串长度
  * ` char charAt(int index) `：获取指定位置索引的字符
  * ` int indexOf(int ch) `：返回指定字符在字符串中第一次出现处的索引
  * ` int indexOf(String str) `：返回支定字符串在此字符串中第一次出现的索引
  * ` int indexOf(int ch,int fromIndex) `：返回指定字符在此字符串从指定位置后第一次出现处的索引
  * ` int indexOf(String str,int fromIndex) `：返回指定字符串在此字符串从指定位置后第一次出现处的索引
  * ` String substring(int start) `：从指定位置截取字符串，默认到尾位
  * ` String substring(int start,int end) `：从指定位置开始到指定位置结束截取字符串

* String类的转换方法
  * ` byte[] getBytes() `：把字符串转换为字节数组
  * ` char[] getCharArray() `：把字符串转换为字符数组
  * ` static String valueOf(char[] chs) `：把字符数组转换为字符串
  * ` static String valueOf(int i) `：把int类型的数据转换成字符串
    * 注：valueOf 可以把任意类型的数据转换成字符串，底层是由String类的构造方法完成的
  * ` String toLowerCase() `：把字符串转成小写
  * ` String toUpperCase() `：把字符串转成大写
  * ` String concat(String str) `：把字符串拼接
    * 注：更常用的是+来拼接，这样字符串可以拼接任意类型，concat方法只能拼接字符串

* String类的其他方法
  * ` String replace(char old,char new) `：用新字符替换原字符串中的旧字符
  * ` String replace(String old,String new) `：用新字符串替换原字符串中的旧字符串
  * ` String trim() `：去除字符串两端空格
  * ` int compareTo(String str) `：按照字典顺序比较两个字符串。如果长度不同，返回长度差值；如果长度相同，按照字典顺序依次比较各个字符。区分大小写
  * ` int compareToIgnoreCase(String str) `：同上，不区分大小写

#### StringBuffer

* StringBuffer是java.lang包下的，使用时不需要导包

* 由final修饰，没有子类

* **线程安全**的可变字符序列

* String是一个不可变的字符序列；StringBuffer是一个可变的字符序列 

* StringBuffer的构造方法

  * ` public StringBuffer() `：构造一个其中不带字符的字符串缓冲区，初始容量16个字符
    * 如果内部缓冲区溢出，此容量自动增大。JDK5开始，为该类补充了一个单线程使用的类，即StringBuilder，因为不执行同步，所以StringBuilder更快
  * ` public StringBuffer(int capacity) `：指定容量的字符串缓冲区对象
  * ` public StringBuffer(String str) `：指定字符串内容的字符串缓冲区对象
    * 创建的字符缓冲对象的容量为：16+字符串长度
  * ` public StringBuffer(CharSequence seq) `：CharSequence是一个接口

* StringBuffer的获取方法

  * ` int capacity() `：返回当前容量（不等同于字符数）
  * ` int length() `:返回长度（字符数）
  * ` String substring(int start)`：从指定位置截取到末尾
    * 注：返回值类型为String
  * `String substring(int start,int end)`：截取从指定位置开始到结束位置，包括开始位置，不包括结束位置
    * 注：返回值类型为String

* StringBuffer的添加删除替换反转

  * ` StringBuffer append(String str) `：可以把任意类型数据（不只是String，详情可以查看[API](https://docs.oracle.com/en/java/javase/13/docs/api/index.html)）添加到字符串缓冲区里面,并返回字符串缓冲区本身

    * 不同于String，String在添加时，是重新创建对象，再将原来的对象当作垃圾处理掉。是因为String对象是不可变的，字符串被储存在常量池中，可以共享
  * `  StringBuffer insert(int offset,String str)`：在指定位置把任意类型的数据插入到字符串缓冲区里面,并返回字符串缓冲区本身

    * 如果没有指定位置的索引，会报索引越界异常
  * ` StringBuffer deleteCharAt(int index) `：删除指定位置的字符，并返回本身
  * ` StringBuffer delete(int start,int end) `：删除从指定位置开始指定位置结束的内容，并返回本身

    * 结束位置的元素不会被删除，即如果start=0，end=2，只会删除位置0跟1的元素，即**包含头部不包含尾部**
    * 删除整个缓冲区:`sb.delete(0,sb.length());` 
  * `  StringBuffer replace(int start,int end,String str)`：从start开始到end用str替换
  * 同delete跟substring方法，包含头部不包含尾
  * `StringBuffer reverse()`：字符串反转

* String与StringBuffer相互转换

  * String转换为StringBuffer
    * 通过构造方法：`StringBuffer sb = new StringBuffer(str);`
    * 通过append()方法：`sb.append(str);`

  * StringBuffer转换为String
    * 通过构造方法：`String str = new String(sb); `
    * 通过toString()方法：`str = sb.toString();`
    * 通过subString(0,length)：`str = sb.subString(0,length);`

* String和StringBuffer作为参数传递

  ```java
  public static void change(String str){				//调用方法不改变原来str的值
  	str += "change";
  }
  
  public static void change(StringBuffer sb){			//调用方法会改变原来sb的值
      sb.append("change")
  }
  ```

  * String类虽然是引用数据类型，但是在当作参数传递时和基本数据类型是一样的

#### StringBuilder

* StringBuilder和StringBuffer中的方法是一模一样的

* StringBuffer是jdk1.0版本的，是**线程安全**的，**效率低**；StringBuilder是jdk1.5版本的，是**线程不安全**的，**效率高**

* String是一个**不可变**的字符序列；StringBuffer,StringBuilder是**可变**的字符序列

#### Arrays

* Arrays是java.util包下的，所以在使用时要导包`import java.util.Arrays;`

* 成员方法
  * `public static String toString(int[] a)`：数组转字符串
  * `public static void sort(int[] a)`：排序
  * `public static int binarySearch(int[] a,int key)`：二分查找，查找有序数组；如果有重复的值，无法确定找到的是哪一个；如果数组中没有此值，则返回**负的插入点减一**

#### Integer

* 在对象中包装了一个基本类型 int 的值

  * `public Integer(int value)`
  * `public Integer(String s)`：将数字字符串转换为Integer，如果不是数字字符串会报数字格式异常错误

* 提供了多个方法，能在 int 类型和 String 类型之间互相转换

  * int转换成String

    * 直接拼接：`s = i + str;`

    * 用String valueOf(int i)方法：`s = str.valueOf(i)`

    * int -- Integer -- String（Integer类的toString方法()）：

      ```java
      Integer i1 = new Integer(i);		//默认i是要转换的int类型数据
      s = i1.toString();					//默认s是要转换成的String类型数据
      ```

    * public static String toString(int i)（Integer类的静态方法）：`s = Integer.toString(i);`

  * String转换成Int

    * String -- Integer -- int：

      ```java
      Integer i1 = new Integer(s);
      i = i1.intValue();
      ```

    * public static int parseInt(String s)：`i = Integer.parseInt(s);`

      * 基本包装类型有八种，七种有parseXxx的方法，可以将其中字符串表形式转换成基本数据类型（只有char的包装类没有，字符串到字符的转换可以通过toCharArray()方法转换）


* 提供了处理 int 类型时非常有用的其他一些常量和方法

  * `String toString()`：返回一个表示该Integer值的String对象
  * `String toBinaryString(int i)`：以二进制无符号整数形式返回一个整数参数的字符串表达形式
  * `MIN_VALUE`：int能够表示的最小值，即  -2^31
  * `MAX_VALUE`：int能够表示的最大值，即  2^31-1

* Test

  ```java
  Integer i1 = new Integer(97);
  Integer i2 = new Integer(97);
  System.out.println(i1 == i2);						//false，比较两个对象，看地址值是否相同
  System.out.println(i1.equals(i2));					//true
  System.out.println("-----------");
  
  Integer i3 = new Integer(197);
  Integer i4 = new Integer(197);
  System.out.println(i3 == i4);						//false
  System.out.println(i3.equals(i4));					//true
  System.out.println("-----------");
  
  Integer i5 = 127;
  Integer i6 = 127;
  System.out.println(i5 == i6);						//true，自动装箱
  System.out.println(i5.equals(i6));					//true
  System.out.println("-----------");
  
  Integer i7 = 128;
  Integer i8 = 128;
  System.out.println(i7 == i8);						//false
  System.out.println(i7.equals(i8));					//true
  
  //-128到127是byte的取值范围，在这个取值范围内，自动装箱，不会创建新的对象，而是从常量池中获取
  //超过了byte的取值范围就会再创建对象
  ```

  


#### 基本类型包装类

| 基本类型 |  包装类   |
| :------: | :-------: |
|   byte   |   Byte    |
|  short   |   Short   |
|   int    |  Integer  |
|   long   |   Long    |
|  float   |   Float   |
|  double  |  Double   |
|   char   | Character |
| boolean  |  Boolean  |

* 只有int和char是不同的，其他都是首字母大写
* 将基本数据类型封装成对象的好处在于可以在对象中定义更多的功能方法操作该数据
* 常用的操作之一：用于基本数据类型与字符串之间的转换
* 从JDK5之后可以**自动装箱**和**自动拆箱**，即基本数据类型和包装类可以自动转换
  * 注：`Integer i = null;`会出现空指针异常，在使用前先要判断是否为null

















