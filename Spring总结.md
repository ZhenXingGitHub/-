## Spring总结 ##
### 一、关于Spring ###
**简介**

	 Spring是一个开放源代码的设计层面框架， 它解决的是业务逻辑层和其他各层的松耦合问题，因此它将面向接口的编程思想贯穿整个系统应用。Spring是于2003 年兴起
	 的一个轻量级的Java 开发框架，由Rod Johnson创建。简单来说，Spring是一个分层的JavaSE/EEfull-stack(一站式) 轻量级开源框架。（百度百科）
**优点**
	
	-轻量： Spring是轻量级的，基本版本大约2MB；
	-控制反转ioc： 又叫依赖注入，Spring通过控制反转实现松散耦合，被调用的对象由Spring依赖注入机制完成，在容器实例化对象时主动将其注入到调用对象。
	-面向切面编程aop： Spring支持面向切面编程，通过将业务逻辑从应用服务（ep：事务管理）中分离，实现高内聚开发，应用对象只关注业务逻辑，不在负责其他系统问
		题（ep：日志、事务等）。支持自定义切面。
	-容器： Spring包含并管理应用中对象的生命周期和配置。
	-MVC框架： Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
	-事务管理： Spring提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务。
	-异常处理： Spring提供方便的API把具体技术相关异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked异常。
### 二、IOC 和 AOP ###
[**IOC**](http://blog.csdn.net/qq_22654611/article/details/52606960)
	
	控制反转，又叫做依赖注入DI，把应用中的代码量降到最低，是应用容易测试，单元测试不再需要单例和JNDI查找机制（no懂）。实现松散耦合。
		依赖注入包括:属性注入、构造方法注入、工厂方法注入、泛型依赖注入。注入的参数包括：属性基本值、注入bean、内部bean、null、级联属性、集合类型属性。
	Spring容器能够自动装配相互合作的bean，这意味着容器不需要<constructor-arg>和<property>配置，能通过Bean工厂自动处理bean之间的协作。
[**AOP**](https://www.cnblogs.com/hongwz/p/5764917.html)
	
	面向切面编程，所谓"切面"，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的
		可操作性和可维护性。
	Spring AOP实例：前台通知、后置通知、环绕通知、返回通知、异常通知。

### 三、Spring对DAO的支持 ###

	降低耦合度，倾向于面向接口编程，简化编程{愈来愈简化}

**Spring对JDBC的支持**
	
**Spring对Hibernate的支持**
	
	-Hibernate使用上Spring的声明式事务
	-由IOC容器来管理Hibernate的SessionFactory，Spring提供了HibernateTemplate，用于持久层访问，该模板无需打开Session及关闭Session。它只要获得
		SessionFactory的引用，将可以只能地打开Session，并在持久化访问结束后关闭Session，程序开发只需完成持久层逻辑，通用的操作（如对数据库中
		数据的增，删，改，查）则有HibernateTemplate完成。
### 四、Spring对事务的支持 ###
	Spring为事务管理提供了一致的编程模板，在高层次建立了统一的事务抽象。也就是说，不管选择Spring JDBC、Hibernate 、JPA 还是iBatis，Spring都让我们
		可以用统一的编程模型进行事务管理。
	Spring支持两种类型的事务管理：编程式事务管理、声明式事务管理

#### 整合Struts、Hibernate、Spring... ####

	


