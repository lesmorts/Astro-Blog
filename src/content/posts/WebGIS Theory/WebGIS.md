---
title: WebGIS理论概述
published: 2023-11-23
comment: false 
tags: [WebGIS]
category: WebGIS
---
# WebGIS

## WebGIS概述

### WebGIS主流结构-分布式服务体系结构

- 客户端:用户使用WebGIS的界面，通过提交请求获得结果
- 服务器端：通过HTTP协议和TCP/IP协议为用户提供信息交换通道和地理信息处理功能
- 空间数据库：存储和管理空间数据

实现该结构的技术：CGI、Java Applet、插件、ActiveX（3D使用较多）、Server API（主流技术）

#### Server API开发方式

- 服务器控件

  客户端浏览器+Web服务器。

  二次开发平台如ArcGIS Server 9.2
- Ajax控件

  客户端浏览器+Ajax引擎+Web服务器

  **使用户操作与服务器响应异步化**：处理用户需求时，仅将必要数据和数据处理提交

  优点：减轻服务器和网络负担

  二次开发平台：ArcGIS Server 9.3

  公共平台：Google Map API

  开源平台：GeoServer、MapServer

### AJAX技术

**通过在后台与服务器进行少量数据交换，使网页实现异步更新**

#### 优势

- 减轻服务器的负担
- 无刷新更新页面
- 可调用外部数据
- Web界面与应用分离

## ASP.NET

### 特点概述

#### ASP优点

- 支持多种语言
- 应用程序运行在公共运行语言运行库
- 面向对象
- 与设备和浏览器无关
- 发布和配置容易

#### ASP缺点

- 仅两种非类型语言：VBScript 和 JavaScript
- 代码、HTML和文本的混合
- 无调试机制

#### ASP.NET优点

- 支持强类型语言如C#、VB.NET
- 页面代码分离
- 编译页可改进执行速度
- 无需注册控件
- 通过继承机制支持代码重用

### 主要功能

把脚本、HTML、组件和Web数据库访问功能有机地结合在一起，形成一个能在服务器端运行的应用程序

### 语法与对象

#### 页面文件（*.aspx）

指令：

1. @Page指令：`<%@ Page Language=”C#” %> `

   属性：Language、Debug、AutoEventWireup（页面事件是否自动绑定）
2. @import指令：`<% @import NameSpace=“值” %>`

语法：

- 使用 `<% %>`或 `<Script></Script>`标记ASP.NET语言
- `<% %>`中只允许内联代码/表达式
- `<Script></Script>`中可进行方法定义和事件响应的编写
- 注释：`<%-- 注释的内容  --%>`

事件处理：

- 页面处理事件
  - page_Init
  - Page_Load
  - Control
  - Page_Unload
- 客户端触发事件

#### 编写代码方式

- 流模式
- `<script>`
- 页面代码分离

#### 内置对象

##### Request

**常用属性：Cookies、Form、QueryString**

涉及页面间使用Request.Form和Request.QueryString传值

###### [示例代码1](D:\Codefield\Language\Code_C#\WebApp\WebApplication1\WebApplication1.sln)

###### [示例代码2](D:\Codefield\Language\Code_C#\WebApp\WebApplication7\WebApplication7.sln)

##### Responce

**常用属性：End、Redirect、Write、Cookies**

涉及使用Request和Responce传递参数

1. 使用 `Response.Redirect`转换页面，并使用 `Server.UrlCode`编码传递参数
2. `Request.Params`中存储传递的参数

   （或使用 `Request.QueryString["name"]`）

```c#
protected void Deliver_Click(object sender, EventArgs e)
{
    Response.Redirect("~/WebForm4.aspx?Param="+Server.UrlEncode(Data.Text));
}
```

```c#
 protected void Page_Load(object sender, EventArgs e)
 {
     if (Request.Params["Param"]!=null)
     {
         Data.Text = Request.Params["Param"].ToString();
     }
     else
     { Data.Text = "未接收参数"; }
 }
```

###### [示例代码](D:\Codefield\Language\Code_C#\WebApp\WebApplication9\WebApplication9.sln)

##### Server

###### 常用属性

- Execute：执行另一个aspx页，执行完该页后再返回本页继续执行
- Transfer：终止当前页的执行，并为当前请求开始执行新页
- HtmlEnCode、HtmlDecode：对HTML编码的字符串进行编/解码，并返回已编/解码的字符串
- MapPath：返回与Web服务器上的指定虚拟路径相对应的物理文件路径

1. 区别Execute和Transfer：前者执行后仍执行后续代码，后者执行后即终止当前页执行。
2. HtmlEnCode、HtmlDecode的使用。
3. 区别Server.MapPath与Request.PhysicalApplicationPath返回路径区别：前者返回文件路径，后者返回文件根目录

###### [示例代码](D:\Codefield\Language\Code_C#\WebApp\WebApplication10\WebApplication10.sln)

##### Session

常用属性：Count、Keys

- 存储用户登录网站时候的信息。当用户在页面之间跳转时，存储在Session对象中的变量不会被清除

###### [示例代码1](D:\Codefield\Language\Code_C#\WebApp\WebApplication11/WebApplication11.sln)

- 实现页面间相加
- 实现页面间传值

###### [示例代码2](D:\Codefield\Language\Code_C#\WebApp\WebApplication12_Session\WebApplication12_Session.sln)

##### [综合示例1_Shop](D:\Codefield\Language\Code_C#\WebApp\WebApplication13_Shop/WebApplication13_Shop.sln)

##### Application

方法：Lock()、Unlock()

事件：Application_Start()、Application_End()

###### [示例代码](D:\Codefield\Language\Code_C#\WebApp\WebApplication1_Application\WebApplication1_Application.sln)

##### ViewState

功能：页面内传值

#### 控件

## AJAX

### 概述

在浏览器与 Web 服务器之间使用异步数据传输（HTTP 请求），使网页从服务器请求少量的信息，而不是整个页面

通过 AJAX，JavaScript 可使用 JavaScript 的 XMLHttpRequest 对象来直接与服务器进行通信，同时在不重载页面的情况下与Web服务器交换数据

包含的技术：

- 服务器端语言：JS处理数据
- XML（可扩展标记语言）：在服务器和客户端之间传递信息
- html和CSS：呈现页面
- DOM（文档对象模型）：实现动态显示与交互

### XMLHttpRequest

#### 方法

- 建立服务器调用

`void open(String method，String url，boolean asyncn，String username，String password)`

其中前两个参数为必要参数

method参数：GET/Post/Put

其中GET比POST快

POST适用于（**无法使用缓存文件**(更新服务器上的文件或数据库)、**向服务器发送大量数据**（POST 没有数据量限制）、**发送包含未知字符的用户输入时**）

boolean参数：决定调用是否为异步

- 向服务器发出请求

`void send（content）`

请求声明为异步则立即返回、否则等待至接受响应

content参数：DOM对象

#### 属性

responceText:返回服务器响应时间的字符串

onreadystatechange：事件处理器，有状态改变时调用一个函数

#### 工作流及编程模式

1.通过事件触发Ajax事件

```html
<input type="text" id="email" name="email" onblur="validateEmail();">
```

2.创建XMLHttpRequest对象实例，调用，设置并触发

```js
Var xmlHttp
function validateEmail(){
    var email=document.getElementById("email")
    var url="validate?email="+escape(email.value)
    if(window.ActiveXObject){
        xmlHttp=new ActiveXObject("Microsoft.XMLHTTP")
    }else if(window.XMLHttpRequest){
        xmlHttp=new XMLHttpRequest()
    }
    xmlHttp.open("GET",url);
    xmlHttp.onreadystatechange=callback;
    xmlHttp.send(null);
}

```

- 用GET传送数据

  `XMLHttpRequest.open(“GET”, “Reponse.aspx?value=1”, true);`

  ` XMLHttpRequest.send(null);`
- 用POST传送数据

  `XMLHttpRequest.open("POST",“re01.aspx",true);`
  `XMLHttpRequest.setRequestHeader("Content-type","application/x-www-form-urlencoded");`
  `XMLHttpRequest.send("fname=Bill&lname=Gates"); `

服务器接收请求后，在ASP.NET中可用QueryString，Form，Params调用传送的数据

服务器响应回浏览器

处理返回数据：XMLHttpRequest对象调用 `callback()`函数检查readyState属性后即进行后续操作

通过使用 `xmlHttp.onreadystatechange=callback;`可以创建一个回调函数

#### 编程模式总结

1. 绑定事件
2. 创建XMLHttpRequest对象
3. 打开与服务器连接open
4. 请求并发送数据
5. 设置回调函数处理服务器返回数据

#### 示例

##### 使用get/post方法传递

仅展现二者区别：

```js
<--get-->
xmlhttp.open("GET","1.aspx?name="+str,true)
xmlhttm.send()

<--post-->
xmlhttp.open("POST","1.aspx",true)
xmlhttps.setRequestHeader("Content-type","application/x-www-form-urlencoded")
xmlhttm.send("name"+str)
```

##### 初始化异步请求并回应

```js
var xmlhttp
function createXMLHttpRequest(){...}//生成XMLHttpRequest对象
function startRequest(){
    createXMLHttpRequest();
    xmlhttp.onreadystatechange=callback
    //通过get打开textFile中文本，并设置异步进行
    xmlhttp.open("GET","TextFile.txt",true)
    xmlhttp.send()
}
function callback(){
    if(...){
       alert("ajax反应："+xmlhttp.responseText)
       }
}
```

##### 连接数据库的登录验证

- 处理输入数据同上
- 服务器端通过 `Request.QueryString["name"]`接收数据
- 由 `SqlConnection`连接数据库，由 `SqlCommand`及方法 `ExecuteScalar()`进行查询
- 根据查询结果通过 `Responce.Write`响应

##### 输入邮编自动填充相应城市省份

- 处理输入的数据
- 回调函数处理接收数据
- 服务器端代码创建表
- 通过 `Request.Params["name"]`接收数据并筛选表中数据
- `Responce.Write`传回处理后数据以响应

##### 通过姓名查询学生信息

- 接收页面端发出数据
- 连接数据库,获得学生信息
- 过 `Request.QueryString["name"]`接收数据并筛选表中数据
- `Responce.Write`传回处理后数据以响应

##### 提示信息

- 接收页面端发出数据
- 遍历服务器端数组，处理响应数据
- `Responce.Write`传回处理后数据以响应

#### 总结AJAX处理步骤

1. 创建XMLHttpRequest对象
2. 注册回调函数
3. 设置连接信息，使用 `open`方法与服务器建立连接
4. 发送数据，与服务端进行交互

   同步下，send执行至服务器设置后完成

   异步下，send执行后立即完成
5. 接收响应数据，并通过回调函数处理
