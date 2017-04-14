---
layout:post
title:"转载：Java图形化界面设计——布局管理器之BorderLayout（边界布局） "
categories:java图形化界面设计
tags:java swing layout
---
*content
{:toc}

转载：http://blog.csdn.net/liujun13579/article/details/7772215

边界布局管理器把容器的的布局分为五个位置：CENTER、EAST、WEST、NORTH、SOUTH。依次对应为：上北（NORTH）、下南（SOUTH）、左西（WEST）、右东（EAST），中（CENTER），如下图所示。







![](http://my.csdn.net/uploads/201207/22/1342932338_7774.png)
特征：

* 可以把组件放在这五个位置的任意一个，如果未指定位置，则缺省的位置是CENTER,它是窗口、框架的内容窗格和对话框等的缺省布局。

* 南、北位置控件各占据一行，控件宽度将自动布满整行。东、西和中间位置占据一行;若东、西、南、北位置无控件，则中间控件将自动布满整个屏幕。若东、西、南、北位置中无论哪个位置没有控件，则中间位置控件将自动占据没有控件的位置。

### 常见的构建函数和方法

**构造方法摘要**

BorderLayout(): 构造一个组件之间没有间距(默认间距为0像素)的新边框布局。

BorderLayout(int hgap, int vgap) :  构造一个具有指定组件（hgap为横向间距，vgap为纵向间距）间距的边框布局。

**方法摘要**

int getHgap() ：返回组件之间的水平间距。

int getVgap() ：返回组件之间的垂直间距。

void removeLayoutComponent(Component comp)：从此边框布局中移除指定组件。

void setHgap(int hgap)：设置组件之间的水平间距。

void setVgap(int vgap) ：设置组件之间的垂直间距。

### 实例

### 实例一

```
// BorderLayoutDemo.Java

import javax.swing.*;

import java.awt.*;

public class BorderLayoutDemo extends JFrame {

  public BorderLayoutDemo(){        //构造函数，初始化对象值

     //设置为边界布局，组件间横向、纵向间距均为5像素

setLayout(new BorderLayout(5,5)); 

     setFont(new Font("Helvetica", Font.PLAIN, 14));

     getContentPane().add("North", new JButton("North"));     //将按钮添加到窗口中

     getContentPane().add("South", new JButton("South"));

     getContentPane().add("East",  new JButton("East"));

     getContentPane().add("West",  new JButton("West"));

     getContentPane().add("Center",new JButton("Center"));

  }  

  public static void main(String args[]) {

     BorderLayoutDemo f = new BorderLayoutDemo();

       f.setTitle("边界布局");

       f.pack();

       f.setVisible(true);

            f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

            f.setLocationRelativeTo(null);             //让窗体居中显示

    }

  }
```

程序执行结果如下所示：
http://my.csdn.net/uploads/201207/22/1342932343_3962.png

依次注释掉东、西、南、北和中间位置添加按钮的语句，保留其它的的语句体会一下边框布局的特点。

如果想要更复杂的布局可以在东、西、南、北和中间位置添加中间容器，中间容器中再进行布局，并添加相应的组件，已达到复制补间的效果。


#### 实例二  在中间位置中添加9个按钮。
```


// BorderLayoutDemo1.java

import javax.swing.*;

import java.awt.*;

public class BorderLayoutDemo1 extends JFrame {

         JPanel p=new JPanel();

  public BorderLayoutDemo(){

     setLayout(new BorderLayout(5,5));

     setFont(new Font("Helvetica", Font.PLAIN, 14));

     getContentPane().add("North", new JButton("North"));

     getContentPane().add("South", new JButton("South"));

     getContentPane().add("East",  new JButton("East"));

     getContentPane().add("West",  new JButton("West"));

         //设置面板为流式布局居中显示，组件横、纵间距为5个像素

          p.setLayout(new FlowLayout(1,5,5));

          //使用循环添加按钮，注意每次添加的按钮对象名称都是b

//但按钮每次均是用new新生成的，所有代表不同的按钮对象。

          for(int i=1;i<10;i++){

                    //String.valueOf(i)，将数字转换为字符串

                   JButton b=new JButton(String.valueOf(i));

                   p.add(b);           //将按钮添加到面板中

          }

     getContentPane().add("Center",p);  //将面板添加到中间位置

  }  

  public static void main(String args[]) {

     BorderLayoutDemo1 f = new BorderLayoutDemo1();

       f.setTitle("边界布局");

       f.pack();  //让窗体自适应组建大小

       f.setVisible(true);

            f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

            f.setLocationRelativeTo(null);             //让窗体居中显示

    }

  }
  ```
  
  程序执行效果
  ![](http://my.csdn.net/uploads/201207/22/1342932348_2751.png)