### SSM总结 ###

----------

#### SSM结合Spring、SpringMvc、Mybatis ####
**Spring：**
	
	Spring对整个项目的bean进行封装，在配置文件中利用AOP面向切面、IOC控制反转和事务管理对可以找到指定的类，设置指定的参数哦和构造方法来实例化构造对象，不需要
		一个一个new。

**SpringMvc:**

	利用SpringMvc的执行机制，拦截用户所有的请求，核心servlet的DispatcherServlet将请求通过HandlerMapping找到响应的Controller，Controller处理以后返回视图
		和模型信息，响应结果返回客户端。

**Mybatis：**

	Mybatis是对JDBC的封装，它让数据库底层操作变的透明。MyBatis应用程序根据XML配置文件创建SqlSessionFactory，SqlSessionFactory在根据配置，配置来源于两个地方，
		一处是配置文件，一处是Java代码的注解，获取一个SqlSession。SqlSession包含了执行sql所需要的所有方法，可以通过SqlSession实例直接运行映射的sql语句，完成
		对数据的增删改查和事务提交等，用完之后关闭SqlSession。
	

----------
**web.xml**

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
	  <display-name>SSM_test</display-name>
	  <welcome-file-list>
	    <welcome-file>login.jsp</welcome-file>
	  </welcome-file-list>
	  
	    <!-- Spring配置文件 -->
		<context-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:applicationContext.xml</param-value>
		</context-param>
		<!-- 编码过滤器 -->
		<filter>
			<filter-name>encodingFilter</filter-name>
			<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
			<async-supported>true</async-supported>
			<init-param>
				<param-name>encoding</param-name>
				<param-value>UTF-8</param-value>
			</init-param>
		</filter>
		<filter-mapping>
			<filter-name>encodingFilter</filter-name>
			<url-pattern>/*</url-pattern>
		</filter-mapping>
		<!-- Spring监听器 -->
		<listener>
			<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
		</listener>
		<!-- 添加对springmvc的支持 -->
		<servlet>
			<servlet-name>springMVC</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
			<init-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>classpath:spring-mvc.xml</param-value>
			</init-param>
			<load-on-startup>1</load-on-startup>
			<async-supported>true</async-supported>
		</servlet>
		<servlet-mapping>
			<servlet-name>springMVC</servlet-name>
			<url-pattern>*.do</url-pattern>
		</servlet-mapping>
	</web-app>
**application.xml**

	<?xml version="1.0" encoding="UTF-8"?>    
	<beans xmlns="http://www.springframework.org/schema/beans"    
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
	    xmlns:p="http://www.springframework.org/schema/p"  
	    xmlns:aop="http://www.springframework.org/schema/aop"   
	    xmlns:context="http://www.springframework.org/schema/context"  
	    xmlns:jee="http://www.springframework.org/schema/jee"  
	    xmlns:tx="http://www.springframework.org/schema/tx"  
	    xsi:schemaLocation="    
	        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd  
	        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd  
	        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd  
	        http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee-4.0.xsd  
	        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd">    
	        
		<!-- 自动扫描 -->
		<context:component-scan base-package="com.xlz.dao" />
		<context:component-scan base-package="com.xlz.service" />
		
		<!-- 配置数据源 -->
		<bean id="dataSource"
			class="org.springframework.jdbc.datasource.DriverManagerDataSource">
			<property name="driverClassName" value="com.mysql.jdbc.Driver"/>
			<property name="url" value="jdbc:mysql://localhost:3306/hello_maven"/>
			<property name="username" value="root"/>
			<property name="password" value=""/>
		</bean>
	
		<!-- 配置mybatis的sqlSessionFactory -->
		<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
			<property name="dataSource" ref="dataSource" />
			<!-- 自动扫描mappers.xml文件 -->
			<property name="mapperLocations" value="classpath:com/xlz/mappers/*.xml"></property>
			<!-- mybatis配置文件 -->
			<property name="configLocation" value="classpath:mybatis-config.xml"></property>
		</bean>
	
		<!-- DAO接口所在包名，Spring会自动查找其下的类 -->
		<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
			<property name="basePackage" value="com.xlz.dao" />
			<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
		</bean>
	
		<!-- (事务管理)transaction manager, use JtaTransactionManager for global tx -->
		<bean id="transactionManager"
			class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
			<property name="dataSource" ref="dataSource" />
		</bean>
		
		<!-- 配置事务通知属性 -->  
	    <tx:advice id="txAdvice" transaction-manager="transactionManager">  
	        <!-- 定义事务传播属性 -->  
	        <tx:attributes>  
	            <tx:method name="insert*" propagation="REQUIRED" />  
	            <tx:method name="update*" propagation="REQUIRED" />  
	            <tx:method name="edit*" propagation="REQUIRED" />  
	            <tx:method name="save*" propagation="REQUIRED" />  
	            <tx:method name="add*" propagation="REQUIRED" />  
	            <tx:method name="new*" propagation="REQUIRED" />  
	            <tx:method name="set*" propagation="REQUIRED" />  
	            <tx:method name="remove*" propagation="REQUIRED" />  
	            <tx:method name="delete*" propagation="REQUIRED" />  
	            <tx:method name="change*" propagation="REQUIRED" />  
	            <tx:method name="get*" propagation="REQUIRED" read-only="true" />  
	            <tx:method name="find*" propagation="REQUIRED" read-only="true" />  
	            <tx:method name="load*" propagation="REQUIRED" read-only="true" />  
	            <tx:method name="*" propagation="REQUIRED" read-only="true" />  
	        </tx:attributes>  
	    </tx:advice>  
	  
	    <!-- 配置事务切面 -->  
	    <aop:config>  
	        <aop:pointcut id="serviceOperation"  
	            expression="execution(* com.xlz.service.*.*(..))" />  
	        <aop:advisor advice-ref="txAdvice" pointcut-ref="serviceOperation" />  
	    </aop:config>  
	
	</beans>
**spring_mvc.xml**

	<?xml version="1.0" encoding="UTF-8"?>    
	<beans xmlns="http://www.springframework.org/schema/beans"    
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
	    xmlns:p="http://www.springframework.org/schema/p"  
	    xmlns:aop="http://www.springframework.org/schema/aop"   
	    xmlns:context="http://www.springframework.org/schema/context"  
	    xmlns:jee="http://www.springframework.org/schema/jee"  
	    xmlns:tx="http://www.springframework.org/schema/tx"  
	    xsi:schemaLocation="    
	        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd  
	        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd  
	        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd  
	        http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee-4.0.xsd  
	        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd">    
	
		<!-- 使用注解的包，包括子集 -->
		<context:component-scan base-package="com.xlz.controller" />
	
		<!-- 视图解析器 -->
		<bean id="viewResolver"
			class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			<property name="prefix" value="/" />
			<property name="suffix" value=".jsp"></property>
		</bean>
	</beans>  
**mybatis.xml**

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE configuration
	PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
		<!-- 别名 -->
		<typeAliases>
			<package name="com.xlz.entity"/>
		</typeAliases>
	</configuration>
**action动作类**
	
	@Controller
	@RequestMapping("/User")
	public class UserController {
		
		@Resource
		private UserService userService;
		
		@RequestMapping("/login")
		public String login(User user, HttpServletRequest request) {
			boolean t = userService.login(user);
			System.out.println("你" + t);
			if(t) {
				request.getSession().setAttribute("user", user);
				System.out.println("你好");
				return "success";
			}else {
				request.getSession().setAttribute("msg", "登录失败");
				return "login";
			}
		}
	}
**pojo类**

	public class User {
		
		private Integer id;
		private String userName;
		private String password;
		
		public Integer getId() {
			return id;
		}
		public void setId(Integer id) {
			this.id = id;
		}
		public String getUserName() {
			return userName;
		}
		public void setUserName(String userName) {
			this.userName = userName;
		}
		public String getPassword() {
			return password;
		}
		public void setPassword(String password) {
			this.password = password;
		}
	}

**service服务类**
	
	package com.xlz.service;
	
	import com.xlz.entity.User;
	
	public interface UserService {
		
		public boolean login(User user);
	}//提供接口

	//响应接口
	@Service
	public class UserServiceImpl implements UserService {
	
		@Resource
		private UserDao userDao;
		
		@Override
		public boolean login(User user) {
			// TODO Auto-generated method stub
			List<User> userList = userDao.login(user);
			if(userList.size()==1)
				return true;
			else
				return false;
		}
	}
**dao层**
	public interface UserDao {
		
		public List<User> login(User user);
	
	}//提供接口
**mapper层**

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="com.xlz.dao.UserDao">
	
		<resultMap type="User" id="UserResult">
			<result property="id" column="id"/>
			<result property="userName" column="userName"/>
			<result property="password" column="password"/>
		</resultMap>
		
		<select id="login" parameterType="User" resultMap="UserResult">
			select * from t_user where userName=#{userName} and password=#{password}
		</select>
	
	</mapper> 
