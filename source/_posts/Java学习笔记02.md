---
title: Java学习笔记02
date: 2019-12-20 17:01:55
tags:
	- Java
	- 基础
	- 选择
	- 循环
categories: Java学习笔记
---

### 选择结构

#### if

* 与三元运算符相比较：三元运算符是一个运算符，运算符操作完毕就应该有一个结果，不能是一个输出

<!--more-->


#### switch

* 格式：

``` java
switch(表达式) {
	      case 值1：
			语句体1;
			break;
		    case 值2：
			语句体2;
			break;
		    …
		    default：	
			语句体n+1;
			break;
    }
```

* case 后边要跟常量值，例如byte、short、char、int等可以提升为int的数据类型（long不可以），string也可以跟在case后。
* break 最后一个可以省略，其他的不省略会出现*case穿透*，建议**不省略**。
* default 不一定需要写，也不一定非得写在最后。


### 循环结构

#### for

* 格式：
  for(初始化表达式;条件表达式;循环后的操作表达式) {
  	循环体;
  }


#### while

* 格式：
  * 格式1
    while(判断条件语句) {
    循环体语句;
    }
  * 格式2
    初始化语句;
    while(判断条件语句) {
    循环体语句;
    控制条件语句;
    }

#### do ··· while

* 格式
  do {    
  循环体语句;    
  控制条件语句;}
  while(判断条件语句);

#### 三种循环的区别

* do……while循环至少执行一次循环体。而for,while循环必须先判断条件是否成立，然后决定是否执行循环体语句。
* for语句执行完后初始化表达变量（循环时使用的i）会被释放，不能再使用。
* 如果你想在循环结束后，继续使用控制条件的那个变量，用while循环，否则用for循环。不知道用谁就用for循环。因为变量及早的从内存中消失，可以提高内存的使用效率。


#### 死循环

* while(true){...……}
* for( ; ; ){...……}


#### Demo

* 输出九九乘法表

``` java
class Demo_99 {
	public static void main (String[] args){
		for (int i = 1;i <= 9 ;i++ ) {					//行数
			for (int j = 1;j <= i ;j++ ) {				//列数
				System.out.print(j + "*" + i + "=" + (i * j) + "\t" );
			}
			System.out.println();
		}
	}
}
```

* Tips：
  * System.out.println("*")	输出后自动换行
  * System.out.print("*")		输出后不换行
  * \是转义符号

```
		'\t' ：	tab键的位置
		'\r' ：	return 回车,回到行首
		'\n' ：	newline 换行				//在Windows系统下，要\r\n一起用；Linux系统\n；Mac系统\r
		'\"' ：	转义双引号
		'\'' ：	转义单引号
```

#### 控制跳转语句

* continue：中止本次循环
* break：跳出循环
* 控制跳转标号

``` java
class Demo_Mark {
	public static void main (String[] args) {
		outer: for(int i = 0;i < 10;i++){
			system.out.println("i = " + i);
			inner: for(int j = 0;j < 10;j++){
				system.out.println("j = " + j);
				break outer;		//结束外部循环，不加标号只能结束内部循环	
			}
		}
	}
}
```

* return，break，continue比较
  return是结束方法
  break是跳出循环
  continue是终止本次循环继续下次循环

### 方法

#### 方法的格式

```
修饰符 返回值类型 方法名(参数类型 参数名1,参数类型 参数名2...) {
	方法体语句;
	return 返回值; 
} 
```

#### 方法重载

在同一个类中，方法名相同，参数列表不同。与返回值类型无关。

* 参数个数不同

* 参数类型不同

  

