---
title: Java学习笔记06
date: 2019-12-25 22:25:38
tags:
	- Java
	- 基础
	- 面向对象
	- 修饰符
	- 内部类
	- package
	- import
categories: Java学习笔记
---

### package关键字


* 将字节码(.class)进行分类存放 ，包其实就是文件夹
* 格式：
  * `package 包名;`
  * 多级包用.分开即可，即：`package 包名1.包名2;`
<!--more-->
* 注意事项：
  * package语句必须是程序的**第一条可执行的代码**
  * package语句在一个java文件中**只能有一个**
  * 如果没有package，默认表示无包名

### import关键字

* 让有包的类对调用者可见,不用写全类名了 
  * 全类名形式：`包名1.包名2.类名`
* 格式：`import 包名;`
* 这种方式导入是到类的名称。虽然可以最后写*，但是不建议。
* package,import,class有顺序

### 修饰符

* 修饰符：
  * 权限修饰符：private，默认的，protected，public
  * 状态修饰符：static，final
  * 抽象修饰符：abstract
* 类的修饰符：
  * 权限修饰符：默认修饰符，public
  * 状态修饰符：final
  * 抽象修饰符：abstract
  * **用的最多的就是：public**
* 成员变量的修饰符：
  * 权限修饰符：private，默认的，protected，public
  * 状态修饰符：static，final
  * **用的最多的就是：private**
* 构造方法的修饰符：
  * 权限修饰符：private，默认的，protected，public
  * **用的最多的就是：public**
* 成员方法的修饰符：
  * 权限修饰符：private，默认的，protected，public
  * 状态修饰符：static，final
  * 抽象修饰符：abstract
  * **用的最多的就是：public**
* 其他的组合规则：
  * 成员变量：public static final
  * 成员方法：
    * public static 
    * public abstract
    * public final


* 四种权限修饰符：private  默认  protected  public

| 修饰符访问权限 | 本类 | 同一个包下（子类和无关类） | 不同包下（子类） | 不同包下（无关类） |
| :------------- | :--: | :------------------------: | :--------------: | :----------------: |
| private        |  Y   |                            |                  |                    |
| 默认           |  Y   |             Y              |                  |                    |
| protected      |  Y   |             Y              |        Y         |                    |
| public         |  Y   |             Y              |        Y         |         Y          |

* 例：protected修饰的，只有在**本类**中，在**同一个包**里的其他类以及**不同包下的子类**可以访问

### 内部类

* 在类中定义类,叫做内部类

* 内部类可以直接访问外部类的成员，**包括私有**。

* 外部类要访问内部类的成员，必须创建对象。

  * 外部类名.内部类名 对象名 = 外部类对象.内部类对象;
  * `Outer.Inner oi = new Outer().new Inner();`
  * **new + 类名 --> 实例化对象**

* 静态内部类：成员内部类被静态修饰

  * 访问方式：外部类名.内部类名 对象名 = 外部类名.内部类对象;
  * `Outer.Inner oi = Outer.new Inner();`
  * 通常把new放在最前边：`Outer.Inner oi = new Outer.Inner();`

* 局部内部类:

  * 局部内部类访问局部变量必须用final修饰

* 匿名内部类

  * 内部类的简化写法
  * 前提：存在一个类（具体类或者抽象类）或者接口
  * 格式：
    new 类名或者接口名(){
    		重写方法;
    	};
  * Demo:

  ```java
  interface Inter{
      public void print();
  }
  
  class Outer {
      class Inner implements Inter{				//前提:存在一个类（具体类或者抽象类）或者接口
          public void print(){
              System.out.println("print");
          }
      }
      public void method(){
          //new Inner().print();					//?
          new Inter (){							//实现Inter接口
              public void print(){				//重写抽象方法
                  System.out.println("print");
              }
          }.print();
      }
  }
  ```

  * 实质上是一个继承了该类或者实现了该接口的子类匿名对象。
  * 匿名内部类只针对重写一个方法的时候适用,多个方法调用会很繁琐
  * 开发中:匿名内部类当作参数传递(本质把匿名内部类看作一个对象)
  * Question:

  ```java
  interface Inter { void show(); }
  class Outer {
  //补齐代码 输出HelloWorld
  }
  class OuterDemo {
  	public static void main(String[] args) {
  		Outer.method().show();						//链式编程,每次调用方法后还能继续调用方法,证明返回值是对象
  	}
  }
  ```

  * Answer:

  ```java
  class Outer {
      public static Inter method(){					//返回值是对象
          return new Inter(){
              public void show(){
                  System.out.println("HelloWorld");
              }
          };
      }
  }
  ```

  




  ```

  ```