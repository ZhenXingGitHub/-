## Serlvet详解 ##
#### 一、Servlet详解 ####
	处理请求和发送响应的过程是有一种叫做Servlet的过程完成，并且Servlet是为了解决实际动态页面而衍生的东西。简单来说：Servlet是运行在服务器上的小程序，
	Servlet的重要功能是在于交互式地浏览和修改数据，生成动态Web内容，是为Web开发服务的。

#### 二、Tomcat和Servlet的关系 ####

	Tomcat 是Web应用服务器,是一个Servlet/JSP容器. Tomcat 作为Servlet容器,负责处理客户请求,把请求传送给Servlet,并将Servlet的响应传送回给客户
	.而Servlet是一种运行在支持Java语言的服务器上的组件. Servlet最常见的用途是扩展Java Web服务器功能,提供非常安全的,可移植的,易于使用的CGI替代品.
![](https://i.imgur.com/G9JeMUB.jpg)

	1.Tomcat将http请求文本接收并解析，然后封装成HttpServletRequest类型的request对象，所有的HTTP头数据读可以通过request对象调用对应的方法查询到。
	2.Tomcat同时会要响应的信息封装为HttpServletResponse类型的response对象，通过设置response属性就可以控制要输出到浏览器的内容，然后将response
	  交给tomcat，tomcat就会将其变成响应文本的格式发送给浏览器

#### 三、Servlet的运行过程 ####

	1.客户端发送请求至服务端；
	2.服务端根据web.xml配置文件的servlet相关信息，将请求转发到响应的servlet；
	3.Servlet引擎调用Servlice（）方法，根据request对象中封装的用户请求与数据库进行交互，得到数据后，servlet会将数据封装在response对象中；
	4.Servlet生成响应内容并将其传至服务端（tomcat就会将其变成响应文本的格式发送给浏览器）。响应内容动态生成，通常取决于客户端的请求；
	5.服务器叫响应返回给客户端。

#### 四、Servlet的生命周期 ####
![](https://i.imgur.com/VFpVaSD.jpg)

	1.加载和实例化，在第一次请求Servlet时，Servlet容器将会创建Servlet实例；
	2.初始化，Servlet容器加载完成Servlet后，必须进行初始化，调用init（）方法；
	3.Servlet初始化后，就处于响应服务器的就绪状态，此时当客户端发送请求，就会调用Servlet实例的servlet()方法，并且根据不同请求方式，调用doPost或者doGet方法；
	4.最后，当Server不在需要Servlet是，Servlet容器负责将servlet实例进行销毁，及调用destroy方法实现。
**谨记**：一个请求是可以产生多个servlet的，但由不同的servlet类实例化的，每个servlet类都只被实例化一次，直到应用程序终止或服务器终止。 
	
#### 五、代码分析 ####
#### 	1.Html 客户端 ####
![](https://i.imgur.com/dM7ALrQ.jpg)

####	2.Web.xml配置文件 ####
![](https://i.imgur.com/iv1EsL4.jpg)

####	3.Servlet服务端响应 ####
![](https://i.imgur.com/43Hpar0.jpg)

#### 六、几个重点的对象。ServletConfig、ServletContext，request、response ####

#### 1.ServletConfig ####
	简单点说就是获取web.xml配置的内容,EP:	
		（1）、public String getInitParameter（String name）：返回指定名称的初始化参数值；
		（2）、public Enumeration getInitParameterNames（)：返回一个包含所有初始化参数名的Enumeration对象；
		（3）、public String getServletName()：返回在DD文件中<servlet-name>元素指定的Servlet名称；
		（4）、public ServletContext getServletContext（）：返回该Servlet所在的上下文对象；

#### 2.ServletContext####
	ServletContext,是一个全局的储存信息的空间，服务器开始，其就存在，服务器关闭，其才释放。EP:
		（1）、public String getInitParameter（String name）：返回指定参数名的字符串参数值，没有则返回null；
		（2）、public Enumeration getInitParameterNames()：返回一个包含多有初始化参数名的Enumeration对象；
#### 2.request和response ####
	Request 对象用于接收客户端浏览器提交的数据，而 Response 对象的功能则是将服务器端的数据发送到客户端浏览器。
	解决乱码问题：
		- response.setCharacterEncoding("utf-8”);//设置服务器端的编码，默认是ISO-8859-1；
		- response.setHeader("contentType", "text/html; charset=utf-8”);//通知浏览器服务器发送的数据格式是text/html，并要求浏览器使用utf-8进行解码。
		- response.setContentType("text/html;charset=utf-8”)等同于response.setHeader("contentType", "text/html; charset=utf-8”);并且覆盖
			response.setCharacterEncoding("utf-8”);在开发是只需设置response.setContentType("text/html;charset=utf-8”)即可。

#### 七、B/S和C/S ####
	1.C/S结构，即Client/Server(客户机/服务器)结构，是大家熟知的软件系统体系结构，通过将任务合理分配到Client端和Server端，降低了系统的通讯开销，可以充分利
		用两端硬件环境的优势。早期的软件系统多以此作为首选设计标准。		
	2.B/S结构，即Browser/Server(浏览器/服务器)结构，是随着Internet技术的兴起，对C/S结构的一种变化或者改进的结构。在这种结构下，用户界面完全通过WWW浏览器
	实现，一部分事务逻辑在前端实现，但是主要事务逻辑在服务器端实现，形成所谓3-tier结构。B/S结构，主要是利用了不断成熟的WWW浏览器技术，结合浏览器的多种Script
	语言(VBScript、JavaScript…)和ActiveX技术，用通用浏览器就实现了原来需要复杂专用软件才能实现的强大功能，并节约了开发成本，是一种全新的软件系统构造技术。
	这种结构更成为当今应用软件的首选体系结构。	
	