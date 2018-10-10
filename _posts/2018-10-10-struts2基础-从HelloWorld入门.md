---
title: struts2基础-从HelloWorld入门
date: 2018-10-10 20:47:12
updated: 2018-10-10 20:47:12
description: struts2 HelloWorld示例
categories: 必修
photo: 
tags: 
- java web
music-id: 
password:
math:
---

### 基础
假设你已经建好了你的开发环境，那么现在让我们继续构建第一个Struts2 项目：Hello World 。这个项目的目标是构建一个收集用户名并在用户名后跟随显示“Hello World”的web应用程序。我们需要为每个Struts2 项目构建以下四个组件：  

|序号|名称及描述  |
|--|--|
| 1 |Action（操作）创建一个动作类，包含完整的业务逻辑并控制用户、模型以及视图间的交互。  |
|2|Interceptors（拦截器）这是控制器的一部分，可依据需求创建拦截器，或使用现有的拦截器。|
|3|View（视图）创建一个JSP与用户进行交互，获取输入并呈现最终信息。|
|4|Configuration Files（配置文件）创建配置文件来连接动作、视图以及控制器，这些文件分别是struts.xml、web.xml以及struts.properties。|  

### 创建Aciton类
Action类是Struts2 应用程序的关键，我们通过它实现大部分的业务逻辑。那么让我们在“Java Resources”>“src”的类目下创建一个名称为“HelloWorldAction.java”的java文件夹，使用一个命名为“manson.struts2”并包含以下内容的资源包。  
当用户点击一个URL时，由Action类来响应用户操作。一个或多个Action类的方法被执行，并返回一个字符串结果。基于结果的值，会呈现一个特定的JSP页面。

```
package manson;

public class HelloWorldAction{
	   private String name;

	   public String execute() throws Exception {
	      return "success";
	   }
	   
	   public String getName() {
	      return name;
	   }

	   public void setName(String name) {
	      this.name = name;
	   }
}

```
这是一个非常简单的具有“name”属性的类。对于“name”属性，我们用标准的getter和setter方法，以及一个返回“success”字符串的执行方法。
Struts2 框架将创建一个“HelloWorldAction”类的对象，并调用execute方法来响应用户的动作。你把你的业务逻辑放进execute方法里，最后会返回字符串常量。简单的描述每个URL，你需要实现一个Action类，你也可以用类名直接作为你的动作名，或者如下面内容所示使用 struts.xml 文件映射到其他name上。  

### 创建视图
我们需要一个JSP来呈现最终的信息，当一个预定义动作发生时这个页面将被Struts2 框架调用，并且这个映像会定义到 struts.xml 文件里。那么让我们在你的Eclipse项目的WebContent文件夹里创建以下JSP文件 HelloWorld.jsp 。在project explorer中右键点击WebContent文件夹并选择“New”>“JSP File”。

```
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="s" uri="/struts-tags" %>
<html>
<head>
<title>Hello World</title>
</head>
<body>
   Hello World, <s:property value="name"/>
</body>
</html>
```

Taglib指令告知Servlet容器这个页面将使用Struts2 标签，并且这些标签会被s放在前面。s:property 标签显示Action类“name”属性的值，这个值是使用HelloWorldAction类的 getName() 方法返回的。  

### 创建主页
在WebContent文件夹里，我们还需要创建 index.jsp 文件，这个文件是用作初始的action URL。用户可以通过点击它命令Struts2框架去调用HelloWorldAction类的定义方法并呈现HelloWorld.jsp视图。

```
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
   pageEncoding="ISO-8859-1"%>
<%@ taglib prefix="s" uri="/struts-tags"%>
   <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<title>Hello World</title>
</head>
<body>
   <h1>Hello World From Struts2</h1>
   <form action="hello">
      <label for="name">Please enter your name</label><br/>
      <input type="text" name="name"/>
      <input type="submit" value="Say Hello"/>
   </form>
</body>
</html>
```

上面视图文件里定义的hello action将通过struts.xml文件影射到HelloWorldAction类及其execute方法。当用户点击提交按钮时，将使得Struts2框架运行HelloWorldAction类中的execute方法，并基于该方法的返回值，选择一个适当的视图作为响应进行呈现。
### 配置文件
我们需要一个映像把URL、HelloWorldAction类（模型）以及 HelloWorld.jsp（视图）联系在一起。映像告知Struts2 框架哪个类将响应用户的动作（URL），类里的哪个方法将要执行，以及基于方法所返回的字符串结果，会呈现怎样的视图。
那么接下来让我们创建一个名为 struts.xml 的文件。因为Struts2 要求 strust.xml 文件显示在classes的文件夹里，所以我们要在WebContent/WEB-INF/classes 的文件夹下创建 struts.xml 文件。Eclipse并没有默认创建“classes”文件夹，因此你需要自己创建。在project explorer里右键点击WEB-INF文件夹并选择“New”>“Folder”，你的 struts.xml 文件应该如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
   "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
   "http://struts.apache.org/dtds/struts-2.0.dtd">
<struts>
<constant name="struts.devMode" value="true" />
   <package name="helloworld" extends="struts-default">
     
      <action name="hello"  class="manson.HelloWorldAction" >
            <result name="success">/HelloWorld.jsp</result>
      </action>
   </package>
</struts>
```

这里说几句关于上述的配置文件。这里我们设定常数struts.devMode的值为真，因为我们是在开发环境下工作，需要查看一些有用的日志消息。然后，我们定义一个名为helloworld的数据包。当你想要把你的Actions集合在一起时，创建一个数据包是非常有用的。在我们的示例中，我们命名我们的动作为“hello”，与URL /hello.action保持一致，由HelloWorldAction.class进行备份。HelloWorldAction.class的execute方法就是当URL /hello.action被调用时运行。如果execute方法返回的结果为“success”，那么我们带用户进入HelloWorld.jsp。  

下一步是创建一个web.xml文件，这是一个适用于Struts2 任何请求的接入点。在部署描述符（web.xml）中，Struts2 应用程序的接入点将会定义为一个过滤器。因此我们将在web.xml里定义一个org.apache.struts2.dispatcher.FilterDispatcher 类的接入点，而web.xml文件需要在WebContent的WEB-INF文件夹下创建。Eclipse已经创建了一个基础的web.xml文件，你在创建项目的时候可以使用。那么，让我们参照以下内容做修改：

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns="http://java.sun.com/xml/ns/javaee" 
   xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
   xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
   http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
   id="WebApp_ID" version="3.0">
   
   <display-name>Struts 2</display-name>
   <welcome-file-list>
      <welcome-file>index.jsp</welcome-file>
   </welcome-file-list>
   <filter>
      <filter-name>struts2</filter-name>
      <filter-class>
         org.apache.struts2.dispatcher.FilterDispatcher
      </filter-class>
   </filter>

   <filter-mapping>
      <filter-name>struts2</filter-name>
      <url-pattern>/*</url-pattern>
   </filter-mapping>
</web-app>
```
我们指定了index.jsp作为我们的欢迎文件，那么我们已经配置好了在所有的URL（列如：所有匹配/*模式的URL）上运行Struts2 过滤器。

### 执行应用程序
右键点击项目名称，接着点击“Export”>“WAR File”创建WAR文件，然后将WAR部署到Tomcat的webapps目录中。最后，启动Tomcat服务器并尝试访问URL http://localhost:8080/test/index.jsp ，将会呈现如下图所示的结果：
![https://7n.w3cschool.cn/attachments/tuploads/struts_2/helloworldstruts4.jpg](https://7n.w3cschool.cn/attachments/tuploads/struts_2/helloworldstruts4.jpg)

输入一个“Struts2”值并提交页面，你可以看到以下页面
![https://7n.w3cschool.cn/attachments/tuploads/struts_2/helloworldstruts5.jpg](https://7n.w3cschool.cn/attachments/tuploads/struts_2/helloworldstruts5.jpg)

### 吐槽
今天好烦啊，搞了半天一直是404,最后发现是struts一直写的是structs时，心里真的是一万只草泥马奔腾而过。我竟然一直以为是库的问题，神烦。
