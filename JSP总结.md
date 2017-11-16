## JSP总结 ##
### 一、JSP基本语法 ###
#### 1.JSP简介 ####
	-JSP(Java Server Pages),其根本是一个简化的Servlet设计，它实现了在Java中使用HTML标签。JSP是一种动态网页技术标准，也是JavaEE的标准。JSP和Servlet一样，
		是在服务器端执行的。JSP是在Servlet技术发展之后为了让开发者写html标签更方便而发展起来的技术，JSP实际上就是Servlet。
	-简单说：JSP（Java Server Pages）是JavaWeb服务器端的动态资源。它与html页面的作用是相同的，显示数据和获取数据。
#### 2.常用动态网站开发技术 ####
	-JSP：Java平台，安全性高，适合开发大型的，企业级的，分布式的Web应用程序。
	-ASP.net：.Net平台，简单易学。但是安全性以及跨平台型差。
	-PHP：简单，高效，成本低开发周期端，特别适合中小型企业的Web应用开发。
#### 3.JSP组成 ####
	-JSP指令（page指令：定义JSP页面各种属性、include指令：导入其他JSP页面、taglib指令：标签库显示自定义标签）
	-表达式（在JSP页面中执行的表达式 <%=表达式%>）
	-脚本段（在JSP页面中插入多行java代码 <% Java代码 %>）
	-声明（ 在JSP页面中定义变量或者方法  <%! Java代码 %>，可以声明成员变量并初始化，也可以声明方法或定义方法，同时还可以声明静态代码块。JSP声明中不
		能使用这些隐式对象。）
	-注释和静态内容
#### 4.JSP运行原理及生命周期 ####
![](https://i.imgur.com/R4zAmWN.jpg)


### 二、JSP九大内置对象 ###
	JSP内置对象是Web容器创建的一组对象，不使用new关键字就可以使用的内置对象。包括：application、session、response、
		request、out、Page、pageContext、exception、config。
#### 1.application ####
	application 对象可将信息保存在服务器中，直到服务器关闭，否则application对象中保存的信息会在整个应用中都有效。与session对象相比，application对象
	生命周期更长，类似于系统的“全局变量”
#### 2.session ####
	session 对象是由服务器自动创建的与用户请求相关的对象。服务器为每个用户都生成一个session对象，用于保存该用户的信息，跟踪用户的操作状态。session对象
	内部使用Map类来保存数据，因此保存数据的格式为 “Key/value”。 session对象的value可以使复杂的对象类型，而不仅仅局限于字符串类型。
#### 3.request ####
	request 对象是 javax.servlet.httpServletRequest类型的对象。 该对象代表了客户端的请求信息，主要用于接受通过HTTP协议传送到服务器的数据。
	（包括头信息、系统信息、请求方式以及请求参数等）。request对象的作用域为一次请求。
#### 4.response ####
	response 代表的是对客户端的响应，主要是将JSP容器处理过的对象传回到客户端。response对象也具有作用域，它只在JSP页面内有效。
#### 5.out ####
	out 对象用于在Web浏览器内输出信息，并且管理应用服务器上的输出缓冲区。在使用 out 对象输出数据时，可以对数据缓冲区进行操作，及时清除缓冲区中的残余数据，
	为其他的输出让出缓冲空间。待数据输出完毕后，要及时关闭输出流。
#### 6.Page ####
	page 对象代表JSP本身，只有在JSP页面内才是合法的。 page隐含对象本质上包含当前 Servlet接口引用的变量，类似于Java编程中的 this 指针。
#### 7.pageContext  ####
	pageContext 对象的作用是取得任何范围的参数，通过它可以获取 JSP页面的out、request、reponse、session、application 等对象。pageContext对象的创建
	和初始化都是由容器来完成的，在JSP页面中可以直接使用 pageContext对象。
#### 8.config ####
	config 对象的主要作用是取得服务器的配置信息。通过 pageConext对象的 getServletConfig() 方法可以获取一个config对象。当一个Servlet 初始化时，容器
	把某些信息通过 config对象传递给这个 Servlet。 开发者可以在web.xml 文件中为应用程序环境中的Servlet程序和JSP页面提供初始化参数。
#### 9、exception 对象 ####
	exception 对象的作用是显示异常信息，只有在包含 isErrorPage="true" 的页面中才可以被使用，在一般的JSP页面中使用该对象将无法编译JSP文件。excepation
	对象和Java的所有对象一样，都具有系统提供的继承结构。exception 对象几乎定义了所有异常情况。在Java程序中，可以使用try/catch关键字来处理异常情况； 
	如果在JSP页面中出现没有捕获到的异常，就会生成 exception 对象，并把 exception 对象传送到在page指令中设定的错误页面中，然后在错误页面中处理相应的
	 exception 对象。
	
