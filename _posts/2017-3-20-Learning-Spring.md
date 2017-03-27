---
layout : post
title : Learning Spring 01
categaries : java
tags : java spring maven
---
* content
{ : toc }



* spring官网：[http://projects.spring.io/spring-framework/](http://projects.spring.io/spring-framework/)

* 
* 
* 使用maven加载spring的jar包
* 
* spring tool suite安装[https://spring.io/tools/ggts/all](https://spring.io/tools/ggts/all)
spring tool suite是一个 Eclipse 插件，利用该插件可以更方便的在 Eclipse 平台上开发基于 Spring 的应用
安装过程：
1. Help --> Install New Software...
2. 接下来如图：
![](http://omsc27rax.bkt.clouddn.com/mt511ralbhdunpw1n5icpql37s.png) 
![](http://omsc27rax.bkt.clouddn.com/nxr89ro6jqwljxcjhuzcjfph6k.png)

---

新建maven项目*spring*，如果沒有像图片中展示的那样显示“*Maven Project*”，就在"*New->Other->Maven->Maven Project*"中找到“*Maven Project*”
![](http://omsc27rax.bkt.clouddn.com/yhit0ddgnd8419bm47n0azk273.png)
![](http://omsc27rax.bkt.clouddn.com/aqor5vxszwipbl5un6386r18um.png)
![](http://omsc27rax.bkt.clouddn.com/xaf8nkupw5bchgpw3wyc9eof20.png)
![](http://omsc27rax.bkt.clouddn.com/omewtuptn7oryxbwwdsavyjc68.png)
生成的maven项目目录结构如图所示：
![](http://omsc27rax.bkt.clouddn.com/bqexfpkvf9ksxkt6y6lvaeehoa.png)
App.java是一个模板，可以删除。

---

在`src/main/java`的包`pre.huangjs.spring.spring01`中新建类`Hello.java`和`Main.java`
`Hello.java`功能为显示`hell object`
`Main.java`功能为创建Hello对象，设置属性，调用Hello类中方法
`Hello.java`代码如下：
```java
package pre.huangjs.spring.spring01;

public class Hello {
	private String object;
	
	public void setObject(String object) {
		this.object = object;
	}

	public void sayHello() {
		System.out.println("hello " + this.object);
	}
}
```
`Main.java`代码如下：
```
package pre.huangjs.spring.spring01;

public class Main {
	public static void main(String[] args) {
		// 创建Hello对象
		Hello myHello = new Hello();
		// 为对象的属性赋值
		myHello.setObject("world");

		// 调用对象的方法
		myHello.sayHello();
	}
}
```
运行Main.java显示：
```
hello world
```
修改*pom.xml*文件，添加spring的依赖
![](http://omsc27rax.bkt.clouddn.com/i1ghqjze6rbe280hqiazt660nq.png)
这一部分添加的内容来自spring官网如图位置：
![](http://omsc27rax.bkt.clouddn.com/2017-03-21_19-00-22.jpg)
![](http://omsc27rax.bkt.clouddn.com/c8h9cbxmie8kt616pt797r0xgk.png)

---

在src目录处，点击鼠标右键，新建`Spring Bean Configuration File`
![](http://omsc27rax.bkt.clouddn.com/uw1uy20gzxna513a95bs4xkmrh.png)
![](http://omsc27rax.bkt.clouddn.com/m6ue1liirqmy4bfw84abma07ii.png)
![](http://omsc27rax.bkt.clouddn.com/a99o7bi4rqz1tbsxziuhngva62.png)
在*applicationContext.xml*中添加如下内容
![](http://omsc27rax.bkt.clouddn.com/rqaj072l1bztc7w8s41o4xi529.png)
* id：标志容器中的Bean，必须是唯一的
* class：全类名
* name：类中的属性
* value：为该属性（这里是object）赋值

*src*目录加入classpath
![](http://omsc27rax.bkt.clouddn.com/hc4105m2pdn53rqdvvr3r3vw2v.png)
![](http://omsc27rax.bkt.clouddn.com/mxsgl06zg7jvei5smvjc35hoaj.png)
![](http://omsc27rax.bkt.clouddn.com/s8fe75b17n0l7jru0n45l6noec.png)
修改*Main.java*类
```
package pre.huangjs.spring.spring01;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {
	public static void main(String[] args) {
		/*
		// 创建Hello对象
		Hello myHello = new Hello();
		// 为对象的属性赋值
		myHello.setObject("world");
		*/
		
		// 创建spring的IOC容器对象
		ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
		// 从IOC容器中取出获取Bean实例
		Hello myHello = (Hello)ctx.getBean("hello");
		
		// 调用对象的方法
		myHello.sayHello();
	}
}
```
注释掉之前调用类的方法，使用spring方式（？？），其中，
*ApplicationContext*代表IOC容器
*ClassPathXmlApplicationContext*表示从类路径下加载配置文件，这就是为什么要对*src*进行*Bulid Path*
*getBean()*方法用于取得IOC容器中的Bean

运行显示结果：
```
hello world
```

---

### 依赖注入
spring支持三种依赖注入：
1. 属性注入：通过setter方法进行注入，最常用
2. 构造器注入：通过构造器进行注入
3. 工厂方法注入（很少使用，不推荐）

* 属性注入

之前使用的方式就是属性注入：
```
<property name="object" value="world"></property>
```
也可以把属性写成节点
```
<property name="object">
	<value>world</value>
</property>
```

* 构造器注入

新建*Car.java*

```
package pre.huangjs.spring.spring01;

public class Car {
	private String type;
	private int seat;
	private double price;

	public Car(String type, int seat) {
		super();
		this.type = type;
		this.seat = seat;
	}

	@Override
	public String toString() {
		return "Car [type=" + type + ", seat=" + seat + ", price=" + price + "]";
	}
}
```
修改*applicationContext.xml*文件，添加Bean
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<!-- 配置Bean -->
	<bean id="hello" class="pre.huangjs.spring.spring01.Hello">
		<property name="object">
			<value>world</value>
		</property>
	</bean>
	<!-- 设置一个四座的小汽车对象 -->
	<bean id="car" class="pre.huangjs.spring.spring01.Car">
		<constructor-arg value="小汽车"></constructor-arg>
		<constructor-arg value="4"></constructor-arg>
	</bean>
</beans>
```
新建*Main.java*，添加获取*id*为*car*的Bean
```
package pre.huangjs.spring.spring01;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main01 {
	public static void main(String[] args) {
		ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");

		// 获取id为car的Bean
		Car myCar = (Car) ctx.getBean("car");
		System.out.println(myCar);
```
结果为
```
Car [type=小汽车, seat=4, price=0.0]
```

修改*applicationContext.xml*文件，再配置一个Bean，

```
	<!-- 设置一个售价为100000的卡车对象 -->
	<bean id="car1" class="pre.huangjs.spring.spring01.Car">
		<constructor-arg value="卡车"></constructor-arg>
		<constructor-arg value="100000"></constructor-arg>
	</bean>
```
*注：只是添加几行，所以未给出整个文件内容*

修改*Main01.java*

```
// 获取id为car1的Bean
Car myCar1 = (Car)ctx.getBean("car1");
System.out.println(myCar1);
```
*注：只是添加几行，所以未给出整个文件内容*

运行显示结果为
```
Car [type=小汽车, seat=4, price=0.0]
Car [type=卡车, seat=100000, price=0.0]
```
这里有错误，原本想配置*price*为100000，可是却配置成了*seat*，如何才能达到想要的结果呢？
修改*applicationContext.xml*文件中刚刚配置的Bean，

* 添加type属性

```
	<!-- 设置一个售价为100000的卡车对象 -->
	<bean id="car1" class="pre.huangjs.spring.spring01.Car">
		<constructor-arg value="卡车"></constructor-arg>
		<constructor-arg value="100000" type="double"></constructor-arg>
	</bean>
 ```
 
* 添加name属性（最常用）

```
	<!-- 设置一个售价为100000的卡车对象 -->
	<bean id="car1" class="pre.huangjs.spring.spring01.Car">
		<constructor-arg value="卡车"></constructor-arg>
		<constructor-arg value="100000" name="price"></constructor-arg>
	</bean>
```

其次还有一种方法用来区分重载的构造器，为了说明清楚现在*Car.java*中再写一个构造器
```
	public Car(String type, int seat, double price) {
		super();
		this.type = type;
		this.seat = seat;
		this.price = price;
	}
```

修改*applicationContext.xml*文件，添加一个*index*属性

```
<!-- 设置一个售价为200000的、20座的公交车对象 -->
	<bean id="car2" class="pre.huangjs.spring.spring01.Car">
		<constructor-arg value="公交车"></constructor-arg>
		<constructor-arg value="200000" index="2"></constructor-arg>
		<constructor-arg value="20" index="1"></constructor-arg>
	</bean>
```

上面这一段如果使用直接赋值的方法，代码如下：

```
	<bean id="car2" class="pre.huangjs.spring.spring01.Car">
		<constructor-arg value="公交车"></constructor-arg>
		<constructor-arg value="20"></constructor-arg>
        <constructor-arg value="200000" ></constructor-arg>
	</bea
```
当然需要在*Main01.java*获取id为*car2*的Bean然后输出才能看到结果：
```
		// 获取id为car2的Bean
		Car myCar2 = (Car) ctx.getBean("car2");
		System.out.println(myCar2);
```
结果：
```
Car [type=小汽车, seat=0, price=4.0]
Car [type=卡车, seat=0, price=100000.0]
Car [type=公交车, seat=20, price=200000.0]
```
注意index属性工作过程是这样的：先由**参数个数**选定某一个构造器，然后再依据*index'*标签的顺序来赋值，不能够妄想指定这是设置第一个属性，这是设置第三个属性的，然后依据这个规定去找合适的构造器，例如我想配置一个“售价为400000的大卡车对象”，制定*index=2*想要它来找到第二个构造器（参数列表为：String type, double price），但是会提示错误
```
	<!-- 设置一个售价为400000的大卡车对象 -->
	<bean id="car3" class="pre.huangjs.spring.spring01.Car">
		<constructor-arg value="大卡车"></constructor-arg>
		<constructor-arg value="400000" index=2></constructor-arg>
	</bean>
 ```
 错误提示：
 ```
 ...
Ambiguous argument values for parameter of type [int] - did you specify the correct bean references as arguments?
 ...
 ```
 