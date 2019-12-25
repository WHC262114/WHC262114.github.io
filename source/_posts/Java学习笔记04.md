---
title: Java学习笔记04
date: 2019-12-21 21:05:56
tags:
	- Java
	- 基础
	- 面向对象
	- 封装
categories: Java学习笔记
---

## 面向对象

* 特征：
  * 封装（encapsulation）
  * 继承（inheritance）
  * 多态（polymorphism）

<!--more-->

* 属性
* 行为

### 类

* class
  * 成员变量（属性）：
  * 成员方法（行为）：
* 类：是一组相关的属性和行为的集合
* 对象：是该类事物的具体体现
* 例如：类：学生；对象：具体的某个学生



* 创建对象的格式
  
* 类名 对象名 = new 类名();
  
* 如何使用
  * 对象名.变量名
  * 对象名.方法名(...)

* 在堆中，如果没有任何引用指向该对象，那么该对象就会变成垃圾，Java中有完善的垃圾回收机制，会不定时对其进行回收。

* 成员变量和局部变量
  * 在类中的位置不同：
    * 成员变量：在类中方法外
    * 局部变量：在方法定义中或者方法声明上
  * 在内存中的位置不同：
    * 成员变量：在堆内存(成员变量属于对象,对象进堆内存)
    * 局部变量：在栈内存(局部变量属于方法,方法进栈内存)
  * 生命周期不同
    * 成员变量：随着对象的创建而存在，随着对象的消失而消失
    * 局部变量：随着方法的调用而存在，随着方法的调用完毕而消失
  * 初始化值不同
    * 成员变量：有默认初始化值
    * 局部变量：没有默认初始化值，必须定义，赋值，然后才能使用。
  * 注意事项：
    * 局部变量名称可以和成员变量名称一样，在方法中使用的时候，采用的是就近原则。
    * 基本数据类型变量包括哪些:byte,short,int,long,float,double,boolean,char
    * 引用数据类型变量包括哪些:数组,类,接口,枚举

* 匿名对象：

  * 调用方法：

  ```java
  new car().run();
  ```

  * 只适合对方法的一次调用，节省代码
  * 调用多次时，不适合。匿名对象调用完就是垃圾，被垃圾回收器回收。
  * 匿名对象可以调用属性，但是没有意义，因为调用后就变成垃圾。

### 封装(encapsulation)

* 概念：隐藏对象的属性和实现细节，仅对外提供公共访问方式。
* 好处：隐藏实现细节，提供公共的访问方式；提高了代码的复用性；提高安全性。
* 原则：将不需要对外提供的内容都隐藏起来；把属性隐藏，提供公共方法对其访问。
* private封装形式，提供对应的getXxx()和setXxx()方法。
* this用来区分成员变量和局部变量。this.xxx就是成员变量。

```java
class Demo1_This {
	public static void main(String[] args) {
		Person p1 = new Person();
		p1.setName("张三");
		p1.setAge(23);
		System.out.println(p1.getName() + "..." + p1.getAge());

		Person p2 = new Person();
		p2.setName("李四");
		p2.setAge(24);
		System.out.println(p2.getName() + "..." + p2.getAge());
	}
}

class Person {
	private String name;			//姓名
	private int age;				//年龄
	
	public void setAge(int age) {		//设置年龄
		if (age > 0 && age < 200) {
			//age = age;
            this.age = age;
		}else {
			System.out.println("请回火星吧,地球不适合你");
		}
		
	}

	public int getAge() {			//获取年龄
		return age;
	}

	public void setName(String name) {	//设置姓名
		//name = name;
        this.name = name;
	}

	public String getName() {
		return name;
	}
}
```

### 构造方法

* 作用：给对象的数据(属性)进行初始化

* 构造方法格式特点：
  * 方法名与类名相同(大小也要与类名一致)
  * 没有返回值类型，连void都没有
  * 没有具体的返回值，可以有`return;`语句

* 构造方法不能用对象调用，在创建对象的时候，系统就已经调用了构造方法

* 构造方法的重载：
  * 方法名相同,与返回值类型无关(构造方法没有返回值),只看参数列表
  * 如果我们没有给出构造方法，系统将自动提供一个无参构造方法；如果我们给出了构造方法，系统将不再提供默认的无参构造方法。

* 代码demo：

  ```java
  class Demo3_Person {
  	public static void main(String[] args) {
  		Person p1 = new Person("张三",23);
  		//p1 = new Person("张天一",23);	//这种方式看运行结果貌似是改名了,其实是将原对象变成垃圾
  		System.out.println(p1.getName() + "..." + p1.getAge());
  
  		System.out.println("--------------------");
  		Person p2 = new Person();		//空参构造创建对象
  		p2.setName("李四");
  		p2.setAge(24);
  
  		p2.setName("李鬼");
  		System.out.println(p2.getName() + "..." + p2.getAge());
  	}
  }
  /*
  构造方法
  	给属性进行初始化
  setXxx方法
  	修改属性值
  	这两种方式,在开发中用setXxx更多一些,因为比较灵活
  */
  class Person {
  	private String name;				//姓名
  	private int age;					//年龄
  
  	public Person() {					//空参构造
  	}
  
  	public Person(String name,int age) {//有参构造
  		this.name = name;
  		this.age = age;
  	}
  	
  	public void setName(String name) {	//设置姓名
  		this.name = name;
  	}
  
  	public String getName() {			//获取姓名
  		return name;
  	}
  
  	public void setAge(int age) {		//设置年龄
  		this.age = age;
  	}
  
  	public int getAge() {				//获取年龄
  		return age;
  	}
  }
  
  ```

* 创建对象`Person p = new Person();`的过程（可以画图理解）：
  * Person.class加载进内存(方法区)
  * 声明一个Person类型引用p(栈)
  * 在堆内存创建对象
  * 给对象中属性默认初始化值
  * 属性进行显示初始化
  * 构造方法进栈,对对象中的属性赋值,构造方法弹栈
  * 将对象的地址值赋值给p  


### static 关键字

  * 随着类的加载而加载
  * 优先于对象存在
  * 被类的所有对象共享

  ```java
  class Demo1_Static {
  	public static void main(String[] args) {
  		Person p1 = new Person();
  		p1.name = "苍老师";
  		p1.country = "日本";	
  		
  
  		Person p2 = new Person();
  		p2.name = "小泽老师";	
  		//p2.country = "日本";			//所有对象共享的，就可以定义成静态
  
  		p1.speak();
  		p2.speak();
  
  		Person.country = "日本";	//静态多了一种调用方式,可以通过类名.
  		System.out.println(Person.country);
  	}
  }
  
  class Person {
  	String name;					//姓名
  	static String country;					//国籍
  
  	public void speak() {			//说话的方法
  		System.out.println(name + "..." + country);
  	}
  }
  ```


  * 所有对象共享的，就可以定义成静态
  * 在静态方法中是没有this关键字的
  * 静态方法只能访问静态的成员变量和静态的成员方法：
    * 静态方法：
      * 成员变量：只能访问静态变量
      * 成员方法：只能访问静态成员方法
    * 非静态方法：
      * 成员变量：可以是静态的，也可以是非静态的
      * 成员方法：可是是静态的成员方法，也可以是非静态的成员方法。
    * ***即静态只能访问静态***（记住这句话）


* 静态变量和成员变量的区别
  * 静态变量属于类，也叫类变量 ；成员变量属于对象，也叫对象变量
  * 静态变量存储于方法区的静态区；成员变量存储与堆内存
  * 静态变量随着类的加载而加载，随着类的消失而消失；成员变量随着对象的创建而存在，随着对象的消失而消失
  * 静态变量可以通过类名调用，也可以通过对象调用；成员变量只能通过对象名调用


* 主方法main的格式：
  * `public static void main(String[] args) {}`
  * `public` 被jvm调用，访问权限足够大。
  * `static` 被jvm调用，不用创建对象，直接类名访问
  * `void`被jvm调用，不需要给jvm返回值
  * `main` 一个通用的名称，虽然不是关键字，但是被jvm识别
  * `String[] args` 以前用于接收键盘录入的

### 代码块

在{}内的代码是代码块，分为：局部代码块，构造代码块，静态代码块，同步代码块（多线程）

* 局部代码块：限定变量生命周期，运行结束释放，提高内存利用率
* 构造代码块（初始化块）：在**类**中**方法**外出现；多个构造方法方法中相同的代码存放到一起，**每次调用构造都执行**，并且在**构造方法前执行**
* 静态代码块：在**类**中**方法**外出现，并加上static修饰；用于给类进行初始化，在**加载的时候就执行**，并且**只执行一次**。优先于主方法main执行。一般用于加载驱动。