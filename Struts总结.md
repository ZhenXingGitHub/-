## Struts总结 ##
### 一、Struts简介 ###
	主页：http://struts.apache.org/
	在用户请求和模块化处理方面以及页面的展现这块，Struts2 发挥的屌炸天作用；
	相对于传统的 Jsp+Servlet 模式，Struts2 更适合企业级团队开发，方便系统的维护；
### 二、工作原理 ###
**工作流程**
![](https://i.imgur.com/uhjuqTh.png)

**Struts2处理流程**
![](https://i.imgur.com/x0F3I0E.jpg)
*详情*

	1.	客户端初始化一个Servlet容器（ep：tomcat）的请求
	2.	这个请求经过一系列的过滤器（Filter）
	3.	接着FilterDispacher被调用，FIlterDispatcher询问ActionMapper来决定这个请求是否需要调用某个Action
	4.	如果ActionMapper决定需要调用某个Action，FilterDispatcher把请求的处理交给ActionProxy
	5.	ActionProxy通过Configuration Manager询问框架的配置文件，找到需要调用的Action类
	6.	ActionProxy创建ActionInvocation的实例
	7.	ActionInvocation实例使用命名模式来调用，在调用Action的过程前后，触及相关拦截器Intercepter的调用
	8.	一旦Action执行完毕，ActionInvocation负责根据struts.xml中的配置找到对应的返回结果。返回结果通常是一个需要被表示的JSP或者FreeMarke的
		模板。在表示的过程中可以使用Struts2框架钟继成的标签，在这个过程中需要触及到AcrionMapper
	（以上纯属翻炒；个人简单理解即可。。）

### 三、关于struts###
	- Struts2：struts 采用MVC模式，主要是作用于用户交互，Struts2结合Struts和WebWork，Struts2以WebWork为核心，采用拦截器的机制来处理用户
		的请求，这样的设计也使得业务逻辑控制器能够与ServletAPI完全脱离开。与Struts1不同，Struts2对用户的每一次请求都会创建一个Action，
		所以Struts2中的Action是线程安全的。
	- Struts优点：POJO表单和POJO动作、标签支持、AJAX支持、易于整合、模板支持、插件支持等。
	- 缺点：更大学习曲线（必须是习惯使用标准的JSP，Servlet API和大量精心设计的框架）、欠佳文档、较少透明度。
	- 最后一句：Struts 2是一个最好的网络架构和高度被用于开发富Internet应用程序（RIA）。

### 四、代码配置 ###
**1.Web.xml**
	
	//Struts2在web中的启动配置，框架是通过Filter启动的
	<filter>  
	    <filter-name>struts2</filter-name>  
	    <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>  
	  </filter>   
	  
	  <filter-mapping>  
	    <filter-name>struts2</filter-name>  
	    <url-pattern>/*</url-pattern>  
    </filter-mapping>  

**2.struts.xml**

	<?xml version="1.0" encoding="UTF-8"?>  
	<!DOCTYPE struts PUBLIC  
	    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"  
	    "http://struts.apache.org/dtds/struts-2.0.dtd">  
	<struts>  
	   <package name="itcast" namespace="/test" extends="struts-default">  
			//请求为helloworld 相应包为cn.itcast....HelloWorldAction 实现方法为execute 可以设置为动态请求方式
	        <action name="helloworld" class="cn.itcast.action.HelloWorldAction" method="execute" >  
				//返回结果为success是跳转页面
	    		<result name="success">/WEB-INF/page/hello.jsp</result>  
	        </action>  
	    </package>   
	</struts>  
	//result有多种类型（type），默认是display，其他为 redirect（重定向到jsp页面） 、 redirectAction（重定向跳转到action请求） 、 plainText（普通文本）
		//、chain(将请求转发到一个action)、json（跳转json。。。）、等等。

**3.相关Action.java文件**
	
	//struts为Action提供依赖注入功能（必须提供set...方法）
	package cn.itcast.action;  
	public class HelloWorldAction{  
	    private String message;  
	      
	    public String getMessage() {  
	        return message;  
	    }  
	    public void setMessage(String message) {  
	        this.message = message;  
	    }  
	  
	  	//请求方法 及默认方法
	    public String execute() {  
	        this.message = "我的第一个struts2应用";  
	        return "success";  
	    }  
	}  

### 五、相关struts常量配置 ###

	 <!–该属性设置Struts 2是否支持动态方法调用，该属性的默认值是true。如果需要关闭动态方法调用，则可设置该属性为false。 -->
	 <constant name="struts.enable.DynamicMethodInvocation" value="false"/>
	
	<!-- 指定默认编码集,作用于HttpServletRequest的setCharacterEncoding方法 和freemarker 、velocity的输出 -->
    <constant name="struts.i18n.encoding" value="UTF-8"/>

	 <!--上传文件的大小限制-->
	<constant name="struts.multipart.maxSize" value=“10701096"/>

	 <!-- 该属性指定需要Struts2处理的请求后缀，该属性的默认值是action，即所有匹配*.action的请求都由Struts2处理。
    如果用户需要指定多个请求后缀，则多个后缀之间以英文逗号（,）隔开。 -->
    <constant name="struts.action.extension" value="do"/> //请求后缀为do

	<!-- 设置浏览器是否缓存静态内容,默认值为true(生产环境下使用),开发阶段最好关闭 -->
    <constant name="struts.serve.static.browserCache" value="false"/>

	<!-- 当struts的配置文件修改后,系统是否自动重新加载该文件,默认值为false(生产环境下使用),开发阶段最好打开 -->
    <constant name="struts.configuration.xml.reload" value="true"/>

    <!-- 开发模式下使用,这样可以打印出更详细的错误信息 -->
    <constant name="struts.devMode" value="true" />

     <!-- 默认的视图主题 -->
    <constant name="struts.ui.theme" value="simple" />

    <!– 与spring集成时，指定由spring负责action对象的创建 -->
    <constant name="struts.objectFactory" value="spring" />

### 六、其他 ###
**Struts 文件上传问题**
*html文件*

      <form action="${pageContext.request.contextPath}/upload2/upload2.do" 
          enctype="multipart/form-data" method="post">  //文件上传标志：enctype="multipart/form-data"
        文件:<input type="file" name="image">
            <input type="submit" value="上传" />
    </form>

*struts.xml文件*

	<package name="upload2" extends="struts-default">
        <action name="upload2" class="com.ljq.action.UploadAction2" method="execute">
            <!-- 动态设置savePath的属性值 -->
            <param name="savePath">/images</param>
            <result name="success">/WEB-INF/page/message.jsp</result>
            <result name="input">/upload/upload.jsp</result>
			//拦截器 
            <interceptor-ref name="fileUpload">
                <!-- 文件过滤 -->
                <param name="allowedTypes">image/bmp,image/png,image/gif,image/jpeg</param>
                <!-- 文件大小, 以字节为单位  struts.multipart.maxSize掌控整个项目所上传文件的最大的Size-->
                <param name="maximumSize">1025956</param>
            </interceptor-ref>
            <!-- 默认拦截器必须放在fileUpload之后，否则无效 -->
            <interceptor-ref name="defaultStack" />
        </action>
    </package>

*。。。Action.java*

	// 封装上传文件域的属性
    private File image;
    // 封装上传文件类型的属性
    private String imageContentType;
    // 封装上传文件名的属性
    private String imageFileName;
    // 接受依赖注入的属性
    private String savePath;

    @Override
    public String execute() {
        FileOutputStream fos = null;
        FileInputStream fis = null;
        try {
			//建立输出流 该包括保存路径及包括文件名（可能存在乱码问题）
            fos = new FileOutputStream("文件路径..." + "\\" + getImageFileName());
            // 建立文件上传流
            fis = new FileInputStream(getImage());
			//表示一次最多读1024个字节
            byte[] buffer = new byte[1024];
            int len = 0;
            while ((len = fis.read(buffer)) > 0) {
                fos.write(buffer, 0, len);//将流写入到输出流中
            }
        } catch (Exception e) {
            System.out.println("文件上传失败");
            e.printStackTrace();
        } finally {
			//关闭流
            fis.close();
			fos.close();
        }
        return SUCCESS;
    }

**Struts 文件下载问题**
	
	需要解决中文乱码问题，相关配置很多

**Struts 国际化**
	
	1.	首先在web.xml中配置<constant name="struts.custom.i18n.resources" value="java1234"></constant> 支持国际化 ， java1234 表示后面定义的文件名前缀java1234_。。。。
	2.	资源文件的命名可以是如下3种形式：
                 java1234_language_country.properties
                 java1234_language.properties
                 java1234.properties
			其中java1234是资源文件的基本名称，用户可以自由定义，而language和country都不可随意变化，必须是Java所支持的语言和国家。

	2.	ep:创建文件 支持中文java1234_zh_CN.properties()，下面可以再转为中文是不存在乱码等情况
			userName=\u7528\u6237\u540d (\u7528\u6237\u540d是Unicode编码表示中文的：用户名)
			password=\u5bc6\u7801
			login=\u767b\u5f55
			welcomeInfo=\u6b22\u8fce{0}
		该文件定义的值，可以通过<s:text name=""></s:text> 访问国际化资源 ep:s:text name="login"></s:text> 在不同语言地显示不同，中文下表示登陆 


**Struts防止重复提交**
	
	在struts.xml配置文件中加入两个拦截器：
	<interceptor-ref name="tokenSession"></interceptor-ref>
	<interceptor-ref name="defaultStack"></interceptor-ref>
		需要添加标签<s:actionerror/>

**Struts支持ONGL表达式问题**
