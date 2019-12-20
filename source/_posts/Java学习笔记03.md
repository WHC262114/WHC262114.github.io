---
title: Java学习笔记03
date: 2019-12-20 18:01:55
tags:
	- Java
	- 基础
categories: Java学习笔记
---

### 一维数组

* 数组是一个容器

* 数组可以存储基本数据类型，也可以存储引用数据类型

<!--more-->

* 初始化格式：

  ​	数据类型[] 数组名 = new 数据类型[数组的长度];

  new:创建新的实体或对象

* 动态初始化：只指定长度，由系统给出初始化值

```java
  int[] arr = new int[5]; 
  //系统给默认初始化值：整数类型是0；浮点型：0.0;布尔类型：false;char：\u0000,每个0是一个十六进制的0

```

* 静态初始化：给出初始化值，由系统决定长度

  ​	数据类型[] 数组名 = new 数据类型[]{元素1,元素2,…};

  ​	数据类型[] 数组名 = {元素1,元素2,…};

  区别：第一种格式是先声明，后赋值，可以分开写，分开写赋值时一定要new；第二种格式简写形式，声明跟赋值在同一行。

* 越界和空指针

  * ArrayIndexOutOfBoundsException:数组索引越界异常

    原因：你访问了不存在的索引。

  * NullPointerException:空指针异常

    原因：数组已经不在指向堆内存了。而你还用数组名去访问元素。

    示例：

    ```java
    int[] arr = {1,2,3};
    arr = null;
    System.out.println(arr[0]);
    ```

* 数组的遍历

  arr.length数组的长度；arr.length - 1数组的最大索引

* 数组的反转

  ```java
  public static void reverseArray(int[] arr) {
  				for (int i = 0;i < arr.length / 2 ; i++) {
  					//arr[0]和arr[arr.length-1-0]交换
  					//arr[1]和arr[arr.length-1-1]交换
  					//arr[2]和arr[arr.lentth-1-2]
  					//...
  		
  					int temp = arr[i];
  					arr[i] = arr[arr.length-1-i];
  					arr[arr.length-1-i] = temp;
  				}
  }
  ```

### 二维数组

* 格式1：

  ```java
  //数据类型 数组名[][] = new 数据类型 [m][n];
  //数据类型[] 数组名[] = new 数据类型[m][n];
  
  int[][] arr = new int[3][2]; 
  // arr[] 引用类型数组，默认初始值是null，arr[][]默认初始值是0
  int[] x;
  int[] y[];
  int[] x,y[];	//x是一维数组,y是二维数组
  
  ```

* 格式2：

  ```java
  int[][] arr = new int[3][]; 
  arr[0] = new int [3];
  arr[1] = new int [5];
  arr[2] = new int [8];
  ```

* 二维数组遍历

  嵌套循环

  ```java
  for (int i = 0;i < arr.length ;i++ ) {			//获取到每个二维数组中的一维数组
  	for (int j = 0;j < arr[i].length ;j++ ) {	//获取每个一维数组中的元素
  	System.out.print(arr[i][j] + " ");
  	}
      
  System.out.println();
  }
  ```

  

### Java中的内存分配

* A:栈
  * 存储局部变量 ：定义在**方法**声明上和方法中的**变量**
* B:堆
  * 存储new出来的数组或对象 
* C:方法区
  * 面向对象部分讲解 （代码）
* D:本地方法区
  * 和系统相关 
* E:寄存器
  * 给CPU使用
* 栈、堆示意图：



* 示例代码1：

```java
int arr1 [] = new int[3];
int arr2 [] = new int[5];
int arr3 [] = arr2;

arr1[0] = 10;
arr2[0] = 20;
arr3[0] = 30;

System.out.println(arr2[0]);
System.out.println(arr3[0]);
```

​	输出的结果是30，30，因为此时栈中的arr2跟arr3同时指向了堆中的一个地址（同一个实体），在arr3[0]的赋值中覆盖了上一次arr2[0]的赋值。*（深拷贝与浅拷贝）*

* 基本数据类型值传递跟引用数据类型值传递

```java
public static void main(String[] args) {
		int a = 10;
		int b = 20;
		System.out.println("a:"+a+",b:"+b);
		change(a,b);						//不改变原值，调用完成之后会弹栈，局部变量随之消失
		System.out.println("a:"+a+",b:"+b);
		int[] arr = {1,2,3,4,5};
		change(arr);						//改变原值，即使方法弹栈，堆里的数组对象还在，可以通过地址继续访问
		System.out.println(arr[1]);
}

public static void change(int a,int b) {
	System.out.println("a:"+a+",b:"+b);
	a = b;
	b = a + b;
	System.out.println("a:"+a+",b:"+b);
}

public static void change(int[] arr) {
	for(int x=0; x<arr.length; x++) {
		if(arr[x]%2==0) {
			arr[x]*=2;
		}
	}
}
```

* Java中是传值还是传址

  Java中只是传值，因为地址值也是值。

