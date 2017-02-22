# JSP内置对象

## 四种属性范围

1. 四种属性范围

- page：仅当前页面内有效（pageContext）
- request：当前页有效+服务器跳转有效
- session：当前页有效+服务器跳转有效+客户端跳转有效
	- 在某一浏览器获得该属性时，打开同一浏览器的另一个窗口同样可以获得，但是打开另外一款浏览器不能获得该属性
	- 关闭浏览器后打开同一款浏览器也不可一获得该属性
- application：将属性保存在服务器上，在重启服务器前，在任何浏览器都可以访问

2. 属性操作方法

- public void setAttribute(String name, Object o)
- public Object getAttribute(String name)
- public void removeAttibute(String name)

