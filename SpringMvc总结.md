## SpringMvc总结 ##
### 一、SpringMvc简介 ###
	Spring MVC是Spring提供的一个强大而灵活的web框架。借助于注解，Spring MVC提供了几乎是POJO的开发模式，使得控制器的开发和测试更加简单。这些控制器一般不直接
		处理请求，而是将其委托给Spring上下文中的其他bean，通过Spring的依赖注入功能，这些bean被注入到控制器中。
### 二、SpringMvc运行原理 ###
![](https://i.imgur.com/9qcyIcO.jpg)

	SpringMVC是一个基于DispatcherServlet的MVC框架，每一个http请求最先访问的都是DispatcherServlet，DispatcherServlet负责转发每一个Request请求给相应的
		HandlerMapping，	DispatcherServlet将请求提交到Controller，处理以后再返回相应的视图(View)和模型(Model)，返回的视图和模型都可以不指定，即可以只返回
		Model或只返回View或都不返回。最后将结果显示到客户端。
### 三、SpringMvc常用接口 ###
**DispatcherServlet接口**
	
	Spring提供前端控制器，所有请求都要经过它哎统一发放，在DispatcherServlet将请求分发给Controller之前，需要借助于Spring提供的HandlerMapping定位到具体的
		Controller。
**HandlerMapping接口**

	能够完成客户端的Controller映射
**Controller接口** 

	实现必须保证线程安全，Controller将处理用户请求，这和Struts Action扮演的角色是一致的。一旦Controller处理完用户请求，则返回ModelAndView对象给
		DispatcherServlet前端控制器，ModelAndView中包含了模型（Model）和视图（View）。 
**ViewResolver接口**
	
	Spring提供视图解析器，在web中查找相应的view对象，将结果返回的客户端页面。

### 四、常用 ###
**注解**
	
	@Controller:
	@RequestMapping：
	@Resource和@Autowired：
	@PathVariable：用于将请求URL中的模板变量映射到功能处理方法的参数上，即取出uri模板中的变量作为参数。ep：
			@RequestMapping("/content/{id}")//传递一个int类型的参数id  
			public ModelAndView content(@PathVariable("id") int id) {
				ModelAndView mav = new ModelAndView();//定义 视图和对象 
				if(id == 1) {
					mav.addObject("article", new Article(1, "文章1", "1 内容"));
				}else {
					mav.addObject("article", new Article(2, "文章2", "2 内容"));
				}
				mav.setViewName("Article/article");
				return mav;
			}
	@RequestParam：主要用于在SpringMVC后台控制层获取参数，类似一种是request.getParameter("name")，它有三个常用参数：defaultValue = "0", required = false,
		 value = "isApp"；defaultValue 表示设置默认值，required 铜过boolean设置是否是必须要传入的参数，value 值表示接受的传入的参数类型。ep：
			@RequestMapping("/preSave")
			public ModelAndView preSave(@RequestParam(value = "id", required = false) String id) {//参数id 可以为空
				// @RequestParam(value="id", required=false) String id 可设置是否可以参数
				ModelAndView mav = new ModelAndView();
				if (id != null) {
					System.out.println("更改：" + id);
					mav.addObject("person", personList.get(Integer.parseInt(id) - 1));
					mav.setViewName("Person/update");
				} else {
					mav.setViewName("Person/add");
				}
				return mav;
			}
	@ResponseBody：该注解用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区。
		使用时机：返回的数据不是html标签的页面，而是其他某种格式的数据时（如json、xml等）使用； ep：
			@RequestMapping("/jsonTest")
			public @ResponseBody List<User> jsonTest() {//转换返回结果
				
				return UserUtil.userList;
			}
**时间格式设置**
	
	方法一： 实体类加注解ep：
				@DateTimeFormat(pattern = "yyyy-MM-dd")  
				private Date receiveAppTime;  
	方法二： 控制Action加上一段代码ep：
				@InitBinder  //使用注解
				public void initBinder(WebDataBinder binder) {  
					SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");  
					dateFormat.setLenient(false);  
					binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, true));  
				}
	方法三： 页面把日期类型转化为字符转 
			jsp页面：
				<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>   
				<fmt:formatDate value="${job.jobtime }" pattern="yyyy-MM-dd HH:mm:ss"/>  
			Freemarker：
				<input id="receiveAppTime" name="receiveAppTime" type="text" value="${(bean.receiveAppTime?string('yyyy-MM-dd'))!}" />


**Rest风格的URL**

**文件上传**

	下方代码有相关上传文件的配置
	响应的action：	
		@RequestMapping("/upload")
		public String upload(@RequestParam("file") MultipartFile[] files) throws Exception{
			RequestAttributes ra = RequestContextHolder.getRequestAttributes();    
	        HttpServletRequest request = ((ServletRequestAttributes)ra).getRequest();   
	        //保存路径
			String filePath = request.getServletContext().getRealPath("/") +"upload/";
			for(MultipartFile file:files){
				//转存问价  以文件名保存
				file.transferTo(new File(filePath+file.getOriginalFilename()));			
			}
			return ...;
		}
		

### 五、代码配置 ###
**web.xml**

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
	  <display-name>SpringMvcTest</display-name>
	  <welcome-file-list>
	    <welcome-file>index.jsp</welcome-file>
	  </welcome-file-list>
	  
	  <!-- 拦截器 拦截所有请求 -->
	 	  <servlet>
			<servlet-name>springmvc</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
			<init-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>classpath:spring-mvc.xml</param-value>
			</init-param>
		</servlet>
		<servlet-mapping>
			<servlet-name>springmvc</servlet-name>
			<url-pattern>*.do</url-pattern>
		</servlet-mapping>
	
		<!-- 乱码解决 -->
		<filter>
			<filter-name>characterEncodingFilter</filter-name>
			<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
			<init-param>
				<param-name>encoding</param-name>
				<param-value>utf-8</param-value>
			</init-param>
		</filter>
		<filter-mapping>
			<filter-name>characterEncodingFilter</filter-name>
			<url-pattern>*.do</url-pattern>
		</filter-mapping>
	</web-app>

**spring-mvc.xml**

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xmlns:p="http://www.springframework.org/schema/p"
	    xmlns:context="http://www.springframework.org/schema/context"
	    xmlns:mvc="http://www.springframework.org/schema/mvc"
	    xsi:schemaLocation="
	        http://www.springframework.org/schema/beans
	        http://www.springframework.org/schema/beans/spring-beans.xsd
	        http://www.springframework.org/schema/mvc
	        http://www.springframework.org/schema/mvc/spring-mvc.xsd
	        http://www.springframework.org/schema/context
	        http://www.springframework.org/schema/context/spring-context.xsd">
		<!-- 使用注解的包，包括子集 -->
	    <context:component-scan base-package="com.xlz"/>
	
		<!-- 注解启动   支持对象与json的转换。 -->
	    <mvc:annotation-driven/>  
	    
	    <!-- 拦截器拦截所以//  如下将 所有img 转化为resources文件   请求resource  为img-->
	    <mvc:resources mapping="/resources/**" location="/imgs/"/>
	    
	    <!-- 视图解析器  -->
		<bean id="viewResolver"
			class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			<property name="prefix" value="/WEB-INF/jsp/" />
			<property name="suffix" value=".jsp"></property>
		</bean>
		
		<!-- 上传文件 解释器 -->
		<bean id="multipartResolver"
	        class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
			
			<property name="defaultEncoding" value="UTF-8"/>  
		    <property name="maxUploadSize" value="10000000"/>
	
		</bean>
	
	</beans>
	
