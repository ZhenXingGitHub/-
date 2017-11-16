# SSH总结 #

###  分析  ###
- **Struts2**：struts 采用MVC模式，主要是作用于用户交互，Struts2结合Struts和WebWork，Struts2以WebWork为核心，采用拦截器的机制来处理用户的请求，这样的设计也使得业务逻辑控制器能够与ServletAPI完全脱离开。![](https://i.imgur.com/XTu4nCQ.png) 
	- Struts优点：POJO表单和POJO动作、标签支持、AJAX支持、易于整合、模板支持、插件支持等。
	- 缺点：更大学习曲线（必须是习惯使用标准的JSP，Servlet API和大量精心设计的框架）、欠佳文档、较少透明度。
	- 最后一句：Struts 2是一个最好的网络架构和高度被用于开发富Internet应用程序（RIA）。
- **Spring**：采用IOC和AOP~作用比较抽象，是用于项目的松耦合；Spring是一个轻量级的DI和AOP容器框架，重要是：Spring非侵入式，基于Spring开发的应用一般不依赖Spring类。
	- DI:依赖注入（控制反转IOC），及被调用的对象由Spring依赖注入机制完成，在容器实例化对象时主动将其注入到调用对象。
	- AOP:面向切面编程。Spring对面向切面对象提供支持，通过其将业务逻辑从应用服务（ep：事务管理）中分离，实现高内聚开发，应用对象只关注业务逻辑，不在负责其他系统问题（ep：日志、事务等）。支持自定义切面。
	- 容器:Spring 是个容器，因为它包含并且管理应用对象的生命周期和配置。ep：对象创建、销毁、回掉等。
	- 框架:Spring作为一个框架，提供一些基本功能（ep：事务管理，持久化集成等），是开发人员专注开发应用逻辑，并其对主流框架提供很好的集成支持。
	- 缺点：项目有时会抽风，无缘无故的报错，而且报的错也很神奇。
- **Hibernate**：对象持久化框架（持久化是将程序数据在持久状态和瞬时状态间转换的机制），其实就是实体类和数据库表建立关系，执行相应SQL操作。
	- Hibernate是对JDBC进一步封装。
	- Hibernate的核心：![](https://i.imgur.com/0gSmyre.png)
		- 从上图看出Hibernate六大核心接口，Configuration接口：负责配置并启动Hibernate，SessionFactory接口：负责初始化Hibernate，Session接口：负责持久化对象的CRUD操作，Transaction接口：负责事务，Query和Criteria接口：负责执行各种数据库查询。（Configuration实例是一个启动期间的对象，一旦SessionFactory创建完成它就被丢弃了。）
	- Hibernate三种状态：![](https://i.imgur.com/bKsOdsc.jpg) 
	
	- 优点：以对象的思维操作数据库，只需操作独享，开发更加对象化；代码具有可复用性；轻量级框架，不需要继承任何类和实现任何接口（POJO对象）；代码测试方便；提高工作效率。
	- 缺点：数据库特性语句很难调优；对大批量数据更新存在问题；系统存在大量共计查询功能。
	
### 注解 ###

	- 注入 @Repository、@Service、@Controller 和 @Component 将类标识为Bean
	- @Component 是一个泛化的概念，仅仅表示一个组件 (Bean) ，可以作用在任何层次。
	- @Service 通常作用在业务层，但是目前该功能与 @Component 相同。
	- @Constroller 通常作用在控制层，但是目前该功能与 @Component 相同。
	- @Repository 注解，需要在 XML 配置文件中启用Bean 的自动扫描功能，它用于将数据访问层 (DAO 层 ) 的类标识为 Spring Bean。
	- @Resource（注解属于JavaEE），可以通过name进行指定 装配bean
	- @Autowired（注解属于Spring），和@Resource 效果差不多。。。默认按类型装配，要求一来对象必须存在。

#### 代码配置   使用注解 ####
**web.xml**

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
	  <display-name>GWORD2</display-name>
	  <welcome-file-list>
	    <welcome-file>index.jsp</welcome-file>
	  </welcome-file-list>
	  
	  <!-- 添加对spring的支持 -->  
	  <context-param>  
	    <param-name>contextConfigLocation</param-name>  
	    <param-value>classpath:applicationContext.xml</param-value>  
	  </context-param>  
	    
	    <!-- 定义Spring监听器，加载Spring  -->
	    <listener>  
	        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>  
	    </listener>  
	      
	  <!-- 添加对struts2的支持  拦截器-->  
	  <filter>  
	    <filter-name>struts2</filter-name>  
	    <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>  
	  </filter>   
	  
	  <filter-mapping>  
	    <filter-name>struts2</filter-name>  
	    <url-pattern>/*</url-pattern>  
	  </filter-mapping>  
	  
	  <!-- Session延迟加载到页面  --> 
	   <filter>  
	    <filter-name>openSessionInViewFilter</filter-name>  
	    <filter-class>org.springframework.orm.hibernate4.support.OpenSessionInViewFilter</filter-class>  
	    <init-param>  
	      <param-name>singleSession</param-name>  
	      <param-value>true</param-value>  
	    </init-param>  
	  </filter>  
	    
	   <filter-mapping>  
	    <filter-name>openSessionInViewFilter</filter-name>  
	    <url-pattern>*.action</url-pattern>  
	  </filter-mapping>  
	</web-app>
**struts.xml配置**

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
	  <display-name>GWORD2</display-name>
	  <welcome-file-list>
	    <welcome-file>index.jsp</welcome-file>
	  </welcome-file-list>
	  
	  <!-- 添加对spring的支持 -->  
	  <context-param>  
	    <param-name>contextConfigLocation</param-name>  
	    <param-value>classpath:applicationContext.xml</param-value>  
	  </context-param>  
	    
	    <!-- 定义Spring监听器，加载Spring  -->
	    <listener>  
	        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>  
	    </listener>  
	      
	  <!-- 添加对struts2的支持  拦截器-->  
	  <filter>  
	    <filter-name>struts2</filter-name>  
	    <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>  
	  </filter>   
	  
	  <filter-mapping>  
	    <filter-name>struts2</filter-name>  
	    <url-pattern>/*</url-pattern>  
	  </filter-mapping>  
	  
	  <!-- Session延迟加载到页面  --> 
	   <filter>  
	    <filter-name>openSessionInViewFilter</filter-name>  
	    <filter-class>org.springframework.orm.hibernate4.support.OpenSessionInViewFilter</filter-class>  
	    <init-param>  
	      <param-name>singleSession</param-name>  
	      <param-value>true</param-value>  
	    </init-param>  
	  </filter>  
	    
	   <filter-mapping>  
	    <filter-name>openSessionInViewFilter</filter-name>  
	    <url-pattern>*.action</url-pattern>  
	  </filter-mapping>  
	</web-app>
**hibernate.cfg.xml配置**
	
	<?xml version='1.0' encoding='UTF-8'?>  
	<!DOCTYPE hibernate-configuration PUBLIC  
	         "-//Hibernate/Hibernate Configuration DTD 3.0//EN" 
	    "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">   
	<hibernate-configuration>  
	    <session-factory>  
			
			<!--方言-->
	        <property name="dialect">org.hibernate.dialect.MySQL5Dialect</property>  
	
	            <!-- 控制台显示SQL -->
	        <property name="show_sql">true</property>
	        
	        <property name="format_sql">true</property>
	
	        <!-- 自动更新表结构 -->
	        <property name="hbm2ddl.auto">update</property>
	  
	  
	    </session-factory>  
	</hibernate-configuration>
**application.xml配置**

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
	      
	   
	    <!-- 配置事务切面 -->  
	    <aop:config>  
	        <aop:pointcut id="serviceOperation"  
	            expression="execution(* com.gd.service.impl.*.*(..))" />  
	        <aop:advisor advice-ref="txAdvice" pointcut-ref="serviceOperation" />  
	    </aop:config>  
	  
	    <!-- 自动加载构建bean  base-package为需要扫描的包（含所有子包）  -->  
	    <context:component-scan base-package="com.gd" />  
	  
	</beans>  
**pojo类**

	@Entity
	@Table(name="g_activity")
	public class Activity implements Serializable {
	
		private Integer id;
		private String name;
		private String describes;
		private String bigimg;
		private String smallimg;
		private String declaration;
		private int zhuangtai;//状态  删除和现存  0 1
		private String shijian;
		private Integer fwflow;
	
		@Id
		@GenericGenerator(name = "generator", strategy = "native")
		@GeneratedValue(generator = "generator")
		@Column(name = "id", length=11)
		public Integer getId() {
			return id;
		}
		public void setId(Integer id) {
			this.id = id;
		}
		@Column(name = "name")
		public String getName() {
			return name;
		}
		public void setName(String name) {
			this.name = name;
		}
		@Column(name = "describes")
		public String getDescribes() {
			return describes;
		}
		public void setDescribes(String describes) {
			this.describes = describes;
		}
		@Column(name = "bigimg")
		public String getBigimg() {
			return bigimg;
		}
		public void setBigimg(String bigimg) {
			this.bigimg = bigimg;
		}
		@Column(name = "smallimg")
		public String getSmallimg() {
			return smallimg;
		}
		public void setSmallimg(String smallimg) {
			this.smallimg = smallimg;
		}
		@Column(name = "declaration")
		public String getDeclaration() {
			return declaration;
		}
		public void setDeclaration(String declaration) {
			this.declaration = declaration;
		}
		@Column(name = "zhuangtai")
		public int getZhuangtai() {
			return zhuangtai;
		}
		public void setZhuangtai(int zhuangtai) {
			this.zhuangtai = zhuangtai;
		}
		@Column(name = "shijian")
		public String getShijian() {
			return shijian;
		}
		public void setShijian(String shijian) {
			this.shijian = shijian;
		}
		@Column(name = "fwflow")
		public Integer getFwflow() {
			return fwflow;
		}
		public void setFwflow(Integer fwflow) {
			this.fwflow = fwflow;
		}
		@Override
		public String toString() {
			return "Activity [id=" + id + ", name=" + name + ", describes=" + describes + ", bigimg=" + bigimg
					+ ", smallimg=" + smallimg + ", declaration=" + declaration + ", zhuangtai=" + zhuangtai + ", shijian="
					+ shijian + ", fwflow=" + fwflow + "]";
		}
		
		
		
		
		
	}


**action响应类**

	//使用注解
	@Controller
	public class ActivityAction extends ActionSupport {
		
		@Resource//封装bean 可使用@Autowired 
		private ActivityService activityService;
		
		private Activity activity;
		
		private Page page;
	
		public Page getPage() {
			return page;
		}
	
		public void setPage(Page page) {
			this.page = page;
		}
	
		public Activity getActivity() {
			return activity;
		}
	
		public void setActivity(Activity activity) {
			this.activity = activity;
		}
		
		public String save(){
			
			activityService.save(activity);
			return "save";
		}
		
		public String delete(){//ɾ�������浽ɾ���б�
			
			System.out.println("ɾ�������������������"+activity.getId());
			Activity act = activityService.findById(activity.getId());
			Date now = new Date(); 
			SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
			act.setShijian(dateFormat.format(now));
			act.setZhuangtai(1);
			System.out.println(act);
			activityService.update(act);
			return "activity_delete";
		}
		
		public String list(){//�����б�
			
			System.out.println("adsssssssss");
			List<Activity> activity = activityService.findZT(0);
			System.out.println("dasdasdasda");
			ActionContext.getContext().put("aclist", activity);
			return "activity_list";
		}
		
		。
		。
		。
		。
		。
	}

**Service服务层**
	
	//注解表示为服务层
	@Service("activityService")
	public class ActivityServiceImpl implements ActivityService {
		
		@Resource
		private BaseDao baseDao;
	
		@Override
		public List<Activity> findAll() {
			// TODO Auto-generated method stub
			return this.baseDao.find("from Activity");
		}
	
		。
		。
		。
	}
**Dao层**

	
	//注解表示为dao层
	@Repository("baseDao")
	@SuppressWarnings("all")
	public class BaseDaOImpl<T> implements BaseDao<T>{
	
		private SessionFactory sessionFactory;
	
		public SessionFactory getSessionFactory() {
			return sessionFactory;
		}
	
		@Autowired
		public void setSessionFactory(SessionFactory sessionFactory) {
			this.sessionFactory = sessionFactory;
		}
	
		private Session getCurrentSession() {
			return sessionFactory.getCurrentSession();
		}
	
		public Serializable save(T o) {
			return this.getCurrentSession().save(o);
		}
	
		public void delete(T o) {
			this.getCurrentSession().delete(o);
		}
	
		public void update(T o) {
			this.getCurrentSession().update(o);
		}
	
		public void saveOrUpdate(T o) {
			this.getCurrentSession().saveOrUpdate(o);
		}
		
		public List<T> findByCondition(String hql, Map<String,Object> values) {
			//hql = "from t where 1=1"
			StringBuffer bf=new StringBuffer();
			bf.append(hql);
			for(Entry<String, Object> en:values.entrySet()){
				bf.append(" and "+en.getKey()+"=:"+en.getKey());
			}
			Query q = this.getCurrentSession().createQuery(bf.toString());
			for(Entry<String, Object> en:values.entrySet()){
				q.setParameter(en.getKey(), en.getValue());
			}
			return q.list();
		}
	
		public List<T> find(String hql) {
			return this.getCurrentSession().createQuery(hql).list();
		}
		public T findEnty(String hql) {
			return (T)this.getCurrentSession().createQuery(hql).uniqueResult();
		}
	
		public List<T> find(String hql, Integer rows) {//限制查询条数
			
			if (rows == null || rows < 1) {
				rows = 5;
			}
			Query q = this.getCurrentSession().createQuery(hql);
			
			return q.setMaxResults(rows).list();
		}
		
		public List<T> find(String hql, Object[] param) {
			Query q = this.getCurrentSession().createQuery(hql);
			if (param != null && param.length > 0) {
				for (int i = 0; i < param.length; i++) {
					q.setParameter(i, param[i]);
				}
			}
			return q.list();
		}
	
		public List<T> find(String hql, List<Object> param) {
			Query q = this.getCurrentSession().createQuery(hql);
			if (param != null && param.size() > 0) {
				for (int i = 0; i < param.size(); i++) {
					q.setParameter(i, param.get(i));
				}
			}
			return q.list();
		}
	
		public List<T> find(String hql, Object[] param, Integer page, Integer rows) {
			if (page == null || page < 1) {
				page = 1;
			}
			if (rows == null || rows < 1) {
				rows = 10;
			}
			Query q = this.getCurrentSession().createQuery(hql);
			if (param != null && param.length > 0) {
				for (int i = 0; i < param.length; i++) {
					q.setParameter(i, param[i]);
				}
			}
			return q.setFirstResult((page - 1) * rows).setMaxResults(rows).list();
		}
	
		public List<T> find(String hql, List<Object> param, Integer page, Integer rows) {
			if (page == null || page < 1) {
				page = 1;
			}
			if (rows == null || rows < 1) {
				rows = 10;
			}
			Query q = this.getCurrentSession().createQuery(hql);
			if (param != null && param.size() > 0) {
				for (int i = 0; i < param.size(); i++) {
					q.setParameter(i, param.get(i));
				}
			}
			return q.setFirstResult((page - 1) * rows).setMaxResults(rows).list();
		}
	
		public T get(Class<T> c, Serializable id) {
			return (T) this.getCurrentSession().get(c, id);
		}
	
		public T get(String hql, Object[] param) {
			List<T> l = this.find(hql, param);
			if (l != null && l.size() > 0) {
				return l.get(0);
			} else {
				return null;
			}
		}
	
		public T get(String hql, List<Object> param) {
			List<T> l = this.find(hql, param);
			if (l != null && l.size() > 0) {
				return l.get(0);
			} else {
				return null;
			}
		}
	
		public Long count(String hql) {
			return (Long) this.getCurrentSession().createQuery(hql).uniqueResult();
		}
	
		public Long count(String hql, Object[] param) {
			Query q = this.getCurrentSession().createQuery(hql);
			if (param != null && param.length > 0) {
				for (int i = 0; i < param.length; i++) {
					q.setParameter(i, param[i]);
				}
			}
			return (Long) q.uniqueResult();
		}
	
		public Long count(String hql, List<Object> param) {
			Query q = this.getCurrentSession().createQuery(hql);
			if (param != null && param.size() > 0) {
				for (int i = 0; i < param.size(); i++) {
					q.setParameter(i, param.get(i));
				}
			}
			return (Long) q.uniqueResult();
		}
	
		public Integer executeHql(String hql) {
			return this.getCurrentSession().createQuery(hql).executeUpdate();
		}
	
		public Integer executeHql(String hql, Object[] param) {
			Query q = this.getCurrentSession().createQuery(hql);
			if (param != null && param.length > 0) {
				for (int i = 0; i < param.length; i++) {
					q.setParameter(i, param[i]);
				}
			}
			return q.executeUpdate();
		}
	
		public Integer executeHql(String hql, List<Object> param) {
			Query q = this.getCurrentSession().createQuery(hql);
			if (param != null && param.size() > 0) {
				for (int i = 0; i < param.size(); i++) {
					q.setParameter(i, param.get(i));
				}
			}
			return q.executeUpdate();
		}
	
		
	
	}
	

----------
pojo类也可以使用注解

**待补充**