---
title: Java-web
date: 2022-05-05 11:25:12
tags: ["java","Java-web"]
categories: ["java"]
---
* Tomcat中配置updateresource，改页面后不用重启服务器
* 发送请求的三种方式：1.地址栏 2.超链接（a标签）3.表单（form）
* EL 取get请求中的参数==${param.xx}==    `userServlet?xx=12`

### getClassLoader的路径问题

* JDBCUtils.class.getClassLoader().getResourceAsStream("dbconfig.properties");
* JDBCUtils.class.getResourceAsStream("/dbconfig.properties");==要加/==

### get和post的区别：

* get一般用于从服务器获取数据

* post一般用于向服务器提交数据

* get请求的数据放在URL之后，用？分割，多个参数用&连接

* post请求的数据放在请求体中

* get请求的URL有长度限制，传输的数据量小

* post请求可以传输大量数据，可用于文件传输

* get请求会被浏览器主动缓存，post不会

* 对于参数的数据类型，get只接受ASCII字符，而POST没有限制。

  

### session和cookie的区别：

* session数据存储在服务器，cookie数据存储在客户端
* cookie有数据大小限制，session没有
* session数据相对安全，cookie数据相对不安全
* session可以存储任意类型的数据，cookie只能存储字符串数据



* 跨域：指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对javascript施加的安全限制 。
  
* 同源策略：是指协议，域名，端口都要相同，其中有一个不同都会产生跨域 。

  


### getAttribute和getParameter的区别：

* getParameter获取客户端通过get请求或者post请求发送的数据，返回值为字符串
* getAttribute获取服务器通过setAttribute设置的数据，这样就可以一次请求在多个页面共享数据，返回值为任意类型。

### 获取cookie的方式：

* request中的方法
  * Cookie[] getCookies()
  * String getHeader(String name)

### forword(转发)和redirect(重定向)的区别

* 数据共享方面：forword对于转发页面和转发到的页面可以共享request中的数据。redirect不能共享数据。
* 地址栏显示方面：forword地址栏不变。redirect地址栏显示新URL
* forword是服务器行为。redirect是客户端行为。
* forword一般用于用户登陆的时候,根据角色转发到相应的模块
* redirect:一般用于用户注销登陆时返回主页面和跳转到其它页面
* forword为一次请求，redirect为两次请求。
* redirect可以访问其他服务器的资源，forword只能访问本服务器的资源

* HttpServletResponse是对服务器的响应对象。这个对象中封装了向客户端发送数据、发送响应头，发送响应状态码的方法。

* HttpServletRequest对象代表客户端的请求，当客户端通过HTTP协议访问服务器时，HTTP请求头中的所有信息都封装在这个对象中

### Servlet

* 运行在服务器端的小程序。Servlet就是一个接口，定义了Java类被浏览器访问到的规则
  * 定义一个类，来实现Servlet接口，复写方法
  * 在web.xml中配置Servlet
    * servlet标签、servlet-mapping标签
    * load-on-startup标签，值为负数时：第一次被访问时创建servlet；值为0或正数时：在服务器启动时创建servlet
  * Servlet3.0 使用注解@WebServlet("/servlet")代替web.xml中的配置。


#### 执行原理：

1>.当服务器接收到客户端的请求后，会解析请求URL路径，获取访问的Servlet的资源路径。2>.查找web.xml文件，是否有对应的url-pattern标签体内容。3>.如果有，找到对应的Servlet-class全类名。4>.Tomcat会将字节码文件加载进内存，并且创建其对象。5>.调用其方法。

#### Servlet的生命周期

* 加载、创建、初始化、处理客户请求、卸载。

  (1)加载：容器通过类加载器使用servlet类对应的文件加载servlet。【反射】

  (2)创建：通过调用servlet构造函数创建一个servlet对象

  (3)初始化：调用init方法初始化，只执行一次，Servlet对象是单例的

  (4)处理客户请求：每当有一个客户请求，容器会创建一个线程来处理客户请求。接着调用Servlet的

  Service()方法来响应客户端请求（根据请求的method属性来调用doGet()或者doPost()）

  (5)卸载：服务器正常关闭时，调用destroy方法让servlet自己释放其占用的资源，只执行一次

#### Servlet的体系结构

* Servlet的实现类 GenericServlet：将Servlet接口中的其他方法做了默认实现，只将service()方法定义为抽象方法
  * GenericServlet的子类HttpServlet：复写doGet()/doPost()方法

* HTTP：超文本传输协议

  * 基于TCP/IP协议
  * 默认端口号：80 
  * 基于请求/响应模型：一次请求对应一次响应
  * 无状态的：每次请求之间相互独立，不能交互数据

* Tomcat会创建request对象和response对象，request对象中封装请求消息，通过reponse对象设置响应消息，Tomcat会将这两个对象传递给service方法

  * ServletRequest的子接口HttpServletRequest
  * HttpServletRequest的实现类由Tomcat实现为RequestFacade


#### 请求消息数据格式

* 请求行

  * 请求方式 请求URL 请求协议/版本    GET /ssm/myservlet?name=zhangsan HTTP/1.1
  * 请求方式有7种，常用有2种

* 请求头

  * 请求头名称：请求头值      HOST：localhost

* 请求空行

  * 空行，用于分隔请求头和请求体

* 请求体


##### Request对象

* 获取请求消息数据

  * 获取请求行数据

    GET /ssm/myservlet?name=zhangsan HTTP/1.1

    * 获取请求方式		String getMethod()       get
    * 获取虚拟目录         String getContextpath()       /ssm
    * 获取Servlet路径          String  getServletPath()      /myservlet
    * 获取get方式请求参数            String  getQueryString()          name=zhangsan
    * 获取请求URI        String  getRequestURI()        String  getRequestURL()

  * 获取请求头数据

    * String  getHeader(String name)  通过请求头的名称获取请求头的值
    * Enumeration\<String\> getHeaderNames()  获取所有请求头的名称

  * 获取请求体的数据：只有post请求才有请求体

    * 获取流
      * BufferedReader  getReader()
      * ServletInputStream  getInputStream()
    * 从流中获取数据

* 其他功能

  * 获取请求参数通用方式
    * String  getParameter()  
      * 中文乱码问题：request.setCharacterEncoding("utf-8")
    * String[]  getParameterValues(String name)
    * Enumeration\<String\>  getParameterNames()
    * Map\<String,  String[]\>  getParameterMap()
  * 请求转发
    * request.getRequestDispatcher(path).forward(request,response);
  * 共享数据
    * 域对象：一个有作用范围的对象，可以在范围内共享数据
      * request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
        * void setAttribute(String  name, Object obj)
        * Object  getAttribute(String  name)
        * void  removeAttribute(String  name)
  * 获取ServletContext
    * ServletContext  getServletContext()


#### 响应消息数据格式

* 响应行
  * 协议/版本   响应状态码   状态码描述     HTTP/1.1  200   OK
    * 响应状态码
      * 1XX：服务器收到客户端消息，但没有接收完成，等待一段时间后，发送的状态码
      * 2XX：成功。
      * 3XX：重定向。代表：302（重定向），304（访问缓存）
      * 4XX：客户端错误
      * 5XX：服务器错误
* 响应头
  * 头名称：值       
    * Content-Type：text/html;charset=utf-8
    * Content-disposition：客户端以什么格式打开响应体数据
      * in-line：默认值，在当前页面内打开
      * attachment;filename=xxx.txt   以附件形式打开响应体。
* 响应体


##### Response对象

* 设置响应行    setStatus(int sc)
* 设置响应头    setHeader(String  name,String  value)
* 设置响应体
  * 获取输出流：
    * PrintWriter   getWriter()
    * ServletOutputStream   getOutputStream()
  * 使用输出流，将数据输出到客户端
  * 中文乱码问题：response.setContentType("text/html;charset=utf-8");
* 响应重定向：response.sendRedirect(url);

* 路径问题

  * 相对路径：.开头
  * 绝对路径：/开头
  * 路径给客户端使用，要加虚拟目录；路径给服务器用，不需加虚拟目录


##### ServletContext对象

* 代表整个web应用，可以和程序的容器（服务器）通信

* 获取对象：
  * 通过request获取   request.getServletContext();   
  * 通过HttpServlet获取    this.getServletContext();
* 用于获取MIME类型：在互联网通信过程中定义的一种文件数据类型
  * 格式：  大类型/小类型   text/html       image/jpg
  * String   getMimeType(String file);
* 作用域对象：用于共享数据。范围：所有用户所有请求的数据
  * setAttribute(String  name,Object  value)
  * getAttribute(String  name)
  * removeAttribute(String  name)
* 获取文件的真实（服务器）路径
  * getRealPath(String  fileName)
  * 访问web目录下的资源：fileName为："/a.txt"
  * 访问WEB-INF目录下的资源：fileName为："/WEB-INF/b.txt"
  * 访问src目录下的资源：fileName为："/WEB-INF/classes/c.txt"


#### 会话技术

* 用于在一次会话范围内的多次请求间，共享数据

* 会话：一次会话中包含多次请求和响应。

* 一次会话：客户端第一次给服务器发送请求，会话建立，直到有一方断开为止。
  

##### Cookie客户端会话技术

* 使用步骤：
  * 创建cookie对象，绑定数据。 new  Cookie(String  name,String  value)
  * 发送Cookie对象。response.addCookie(Cookie  cookie)
  * 获取Cookie。request.getCookie()
* 原理：第一次请求时基于响应头set-cookie，下次请求时基于请求头cookie
* 细节：
  * 一次可以发送多个cookie；
  * 默认情况下，浏览器关闭后，cookie数据被销毁。持久化：cookie.setMaxAge(int seconds)
  * Tomcat8之后，cookie支持中文。但是不支持特殊字符，使用URL编码解决[URLEncoder.encode(str,"utf-8")]
  * 默认情况下，同一Tomcat中部署的多个项目间不能共享cookie，通过设置cookie.setPath("/")实现项目间cookie共享。不同Tomcat服务器间cookie共享，通过设置相同的一级域名cookie.setDomain(".baidu.com")，那么tieba.baidu.com和news.baidu.com就可以共享cookie
* cookie的特点：cookie数据存储在浏览器，单个cookie的大小有限制。

##### Session 服务器端会话技术

* 获取Session对象

  * HttpSession  session = request.getSession();

* 使用HttpSession对象

  * Object  getAttribute(String  name)
  * void  setAttribute(String  name,Object  value)
  * void  removeAttribute(String  name)
  * invalidate() 销毁session，用于安全退出

* 原理：通过cookie实现一次会话中多次请求获取的session对象是同一个对象。

* 细节：

  * 客户端关闭，服务器不关闭，两次获取的session默认情况下不同。如果需要相同，可以通过设置键为==JSESSION==的cookie的最大存活时间。

    ```java
  HttpSession session = request.getSession();
    Cookie  cookie = new  Cookie("JSESSION",session.getId());
  cookie.setMaxAge(60*30);//30min
    response.addCookie(cookie);
    ```

    

  * 客户端不关闭，服务器关闭，两次获取的session不同，Tomcat会自动完成以下工作[idea中不会活化]，确保数据不丢失。
  
      * session的钝化：在服务器正常关闭之前，将session对象序列化到硬盘中。
    * session的活化：在服务器启动后，将session文件反序列化到内存中。
  
  * session的什么情况被销毁
  
      * 服务器关闭
      * session对象调用invaliddate()方法
    * session默认失效时间30分钟
* session的特点：session数据存储在服务器，可以存储任意类型、任意大小的数据



### 项目路径问题

* 由浏览器发起的请求，路径中的==“/”==，表示apache-tomcat-8.5.55\webapps 
* 由服务器发起的请求，路径中的=="/"==，表示项目中的web目录
* src目录中的文件编译后形成的class文件在WEB-INF下的classes目录下

* 获取Tomcat中运行项目的路径

  ```java
  //D:\JavaIdea\stage_two_shop\out\artifacts\stage_two_shop_war_exploded\image\
  request.getSession().getServletContext().getRealPath("/image/");
  
  <img src="${pageContext.request.contextPath}/image/db92e2a431af4964b945a0c102e48272.jpg"/>
  ```

### MySQL时间类型与java时间类型

| MySql                                    | java   (java.util.Date是下面类的父类) |
| ---------------------------------------- | ------------------------------------- |
| Date (只包含日期)                        | java.sql.Date                         |
| Datetime (日期和时间)                    | java.sql.Timestamp                    |
| Timestamp （未赋值时自动赋值为当前时间） | java.sql.Timestamp                    |
| Time                                     | java.sql.Time                         |
| Year                                     | java.sql.Date                         |

### SpringMVC数据自动封装问题

```java
public class Employee{
    private String name;
    private Depart depart;
}

public class Depart{
    private Integer id;
    private String name;
}

@PostMapping("/add")
public String addEmp(Employee employee){
    
}

表单中员工所属部门的input的 name="depart.id"
```

前后端分离，ajax返回的结果存在sessionStorage中

```html
//存
sessionStorage.setItem("user",JSON.stringify(res.data));

//取
JSON.parse(sessionStorage.getItem("user"));
```

### sessionStorage

```
var sesion = window.sessionStorage;
session.setItem('name', name);  
var name = session.getItem('name');
```

