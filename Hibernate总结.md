## Hibernate总结 ##
### 一、Hibernate简介 ###
	Hibernate 是一个开放源代码的对象关系映射框架，它对 JDBC 进行了非常轻量级的对象封装，使得 Java程序员可以随心所欲的使用对象编程思维来操纵数据库。 
	  Hibernate 可以应用在任何使用 JDBC 的场合，既可以在Java的客户端程序使用，也可以在Servlet/JSP的Web应用中使用，最具革命意义的是，Hibernate
	  可以在应用 EJB 的 J2EE 架构中取代 CMP，完成数据持久化的重任。ORM 框架，对象关系映射（Object/Relation Mapping）（抄）
	其实就是实体类和数据库表建立关系，执行相应SQL操作。
### 二、Hibernate核心 ###
![](https://i.imgur.com/TzRBSE6.png)

	Configuration接口：负责配置和启动Hibernate
	SessionFactory接口：负责初始化Hibernate及
	Session接口:负责持久化对象的CRUD操作
	Transaction接口：负责开启事务
	Query接口和Criteria接口:负责执行各种数据库查询
	注意：Configuration实例是一个启动期间的对象，一旦SessionFactory创建完成它就被丢弃了。

**其它小记：Hibernate支持逆向工程**
### 三、其他相关 ###

![](https://i.imgur.com/ZFEQlYh.png)

**三种状态**

![](https://i.imgur.com/OjVt6Z0.jpg)

**缓存问题**
		
	-Hibernate中提供了两级缓存，一级缓存是Session级别的缓存，它属于事务范围的缓存，该级缓存由hibernate管理，应用程序无需干预；
	  二级缓存是SessionFactory级别的缓存，该级缓存可以进行配置和更改，并且可以动态加载和卸载，hibernate还为查询结果提供了一个查询缓存，它依赖于二级缓存；
	 （如果二级缓存开启，则首选从二级缓存中查询数据，没有则从一级缓存中查询，如果一级没有再从数据库中查询）
	-开启二级缓存，在hibernate.cfg.xml配置<property name="cache.provider_class">net.sf.ehcache.hibernate.EchCacheProvider</property> ，开启二级
	 缓存需要在。。.hbm.xml中配置 <cache usage="read-only"></cache>

**优点**

	以对象的思维操作数据库，只需操作独享，开发更加对象化；代码具有可复用性；轻量级框架，不需要继承任何类和实现任何接口（POJO对象）；
	 代码测试方便；提高工作效率。

**缺点**

	数据库特性语句很难调优；对大批量数据更新存在问题；系统存在大量共计查询功能。

### 四、代码配置 ###
**hibernate.cfg.xml配置**

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE hibernate-configuration PUBLIC
		"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
		"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
	<hibernate-configuration>
		<session-factory>
			<!-- 配置Hibernate:属性配置参考 Hibernate发型包\project\etc\hibernate.properties -->
			<!-- JDBC的基本链接 -->
			<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
			<property name="connection.username">root</property>
			<property name="connection.password"></property>
			<property name="connection.url">jdbc:mysql://localhost:3306/manageevent</property>
			<!-- 配置数据库方言 -->
			<property name="dialect">org.hibernate.dialect.MySQLDialect</property>
			<!-- 根据映射产生表结构的类型: create-drop:木有表结构创建，下次启动时删除重新创建。适合学习阶段 create：只做创建 update：探测表结构够的变化，对于数据库没有的，进行更新操作。适合学习阶段 
				validate：对比与数据库的结构 -->
			<property name="hibernate.hbm2ddl.auto">update</property>
			<!-- 显示sql语句及格式：开发调试阶段非常有用 -->
			<property name="hibernate.show_sql">true</property>
	
			  <!--  连接池启动时的初始值 --> 
	  		<property name="initialSize">20</property>
			<!--  连接池的最大数据库连接数，即可能的并发量 --> 
			<property name="maxActive">0</property>
			 <!--  连接池中保留的空闲数据库连接的最大数目--> 
			 <!--  当经过一个高峰时间后，连接池可以慢慢将已经用不到的连接慢慢释放一部分，一直减少到maxIdle为止  --> 
			 <property name="maxIdle">50</property>
			<!--  连接池的最小空闲连接数 --> 
			<!--  当空闲的连接数少于阀值时，连接池就会预申请去一些连接，以免洪峰来时来不及申请 --> 
			<property name="minIdle">30</property>
			
			<!-- 映射文件 -->
			<table catalog="manageevent" name="t_kefu"></table>
			
		</session-factory>
	</hibernate-configuration>

**application.xml配置**

	.........查看SSH总结
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
  
	    <!-- 定义数据源 -->
	    <bean id="dataSource"
			class="org.springframework.jdbc.datasource.DriverManagerDataSource">
			<property name="driverClassName"
				value="com.mysql.jdbc.Driver">
			</property>
			<property name="url"
				value="jdbc:mysql://localhost:3306/gword">
			</property>
			<property name="username" value="root"></property>
			<property name="password" value=""></property>
		</bean>
	      
	    <!-- session工厂 -->  
	    <bean id="sessionFactory"  
	        class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">  
	        <property name="dataSource">  
	            <ref bean="dataSource" />  
	        </property>  
	        <property name="configLocation" value="classpath:hibernate.cfg.xml"/>  
	        <!-- 自动扫描注解方式配置的hibernate类文件 -->  
	        <property name="packagesToScan">  
	            <list>  
	                <value>com.gd.model</value>  
	            </list>  
	        </property>  
	        
	    </bean>  
	  
	    <!-- 配置事务管理器 -->  
	    <bean id="transactionManager"  
	        class="org.springframework.orm.hibernate4.HibernateTransactionManager">  
	        <property name="sessionFactory" ref="sessionFactory" />  
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
	      
	   
	    <!-- 配置Spring事务切面 -->  
	    <aop:config>  
	        <aop:pointcut id="serviceOperation"  
	            expression="execution(* com.gd.service.impl.*.*(..))" />  
	        <aop:advisor advice-ref="txAdvice" pointcut-ref="serviceOperation" />  
	    </aop:config>  
	  
	    <!-- 自动加载构建bean  base-package为需要扫描的包（含所有子包）
			Spring的ioc机制（依赖注入）  -->  
	    <context:component-scan base-package="com.gd" />  
  
	</beans>  

**相关.hbm.xml配置文件**

	<?xml version="1.0"?>
	<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
	"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
	<!-- Generated 2017-6-11 9:12:32 by Hibernate Tools 3.2.2.GA -->
	<hibernate-mapping>
	    <class name="com.event.domain.TTalk" table="t_talk" catalog="manageevent">
	        <id name="talkId" type="java.lang.Integer">
	            <column name="talk_id" />
	            <generator class="identity" />
	        </id>
			//实现多对一
	        <many-to-one name="TUserByKefuId" class="com.event.domain.TUser" fetch="select">
	            <column name="kefu_id">
	                <comment>&#191;&#205;&#183;&#254;id</comment>
	            </column>
	        </many-to-one>
	        <many-to-one name="TUserByKehuId" class="com.event.domain.TUser" fetch="select">
	            <column name="kehu_id">
	                <comment>&#191;&#205;&#187;&#167;id</comment>
	            </column>
	        </many-to-one>
	    </class>
	</hibernate-mapping>

**相关执行语句**

	.支持sql语句
	.支持hql语句 ep：from User where userName=？.... User 为pojo类名， name 为变量 （不同于sql语句） 或者直接调用get（）、save（）、find（）。。。。
	.QBC   及实现	Criteria  查询
			ep：DetachedCriteria类
