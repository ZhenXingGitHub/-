## Shiro总结 ##
**[官方参考详解](http://shiro.apache.org/documentation.html)**
****
### 一、简介 ###
	Shiro是一个强大易用的java安全框架，提供认证、授权、加密和会话管理等功能。
### 二、Shiro架构介绍 ###
![](https://i.imgur.com/cAC2p85.png)


**可对数据进行加密      MD5 **
### 三、代码 ###
**web.xml**

	配置shiro过滤器
	 <!-- shiro过滤器定义 -->
		<filter>  
		    <filter-name>shiroFilter</filter-name>  
		    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>  
	    <init-param>  
	    <!-- 该值缺省为false,表示生命周期由SpringApplicationContext管理,设置为true则表示由ServletContainer管理 -->  
	    <param-name>targetFilterLifecycle</param-name>  
	    <param-value>true</param-value>  
	    </init-param>  
		</filter>  
		<filter-mapping>  
		        <filter-name>shiroFilter</filter-name>  
		        <url-pattern>/*</url-pattern>  
		</filter-mapping>

**application.xml**

	<!-- 自定义Realm -->
	<bean id="myRealm" class="com.xlz.realm.MyRealm"/>  
	
	<!-- 安全管理器 -->
	<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">  
  	  <property name="realm" ref="myRealm"/>  
	</bean>  
	
	<!-- Shiro过滤器 -->
	<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">  
	    <!-- Shiro的核心安全接口,这个属性是必须的 -->  
	    <property name="securityManager" ref="securityManager"/>
	    <!-- 身份认证失败，则跳转到登录页面的配置 -->  
	    <property name="loginUrl" value="/login.jsp"/>
	    <!-- 权限认证失败，则跳转到指定页面 -->  
	    <property name="unauthorizedUrl" value="/unauthorized.jsp"/>  
	    <!-- Shiro连接约束配置,即过滤链的定义 -->  
	    <property name="filterChainDefinitions">  
	        <value>  
	             /login=anon
				/admin*=authc
				/student=roles[teacher]
				/teacher=perms["us:create"]
	        </value>  
	    </property>
	</bean>  
	
	<!-- 保证实现了Shiro内部lifecycle函数的bean执行 -->  
	<bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>  
	
	<!-- 开启Shiro注解 -->
	<bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" depends-on="lifecycleBeanPostProcessor"/>  
  		<bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">  
  	  <property name="securityManager" ref="securityManager"/>  
    </bean>  

**Myrealm**

	public class MyRealm extends AuthorizingRealm{
	
		@Resource
		private UserService userService;
		
		/**
		 * 为当限前登录的用户授予角色和权
		 */
		@Override
		protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
			String userName=(String)principals.getPrimaryPrincipal();
			SimpleAuthorizationInfo authorizationInfo=new SimpleAuthorizationInfo();
			authorizationInfo.setRoles(userService.getRoles(userName));
			authorizationInfo.setStringPermissions(userService.getPerms(userName));
			return authorizationInfo;
		}
	
		/**
		 * 验证当前登录的用户
		 */
		@Override
		protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
			String userName=(String)token.getPrincipal();
			User user=userService.getByUserName(userName);
			if(user!=null){
				AuthenticationInfo authcInfo=new SimpleAuthenticationInfo(user.getUserName(),user.getPassword(),"xx");
				return authcInfo;
			}else{
				return null;				
			}
		}
	
	}
**action类**

	@Controller
	@RequestMapping("/User")
	public class UserController {
		/**
		 * 用户登录
		 * @param user
		 * @param request
		 * @return
		 */
		@RequestMapping("/login")
		public String login(User user,HttpServletRequest request){
			Subject subject=SecurityUtils.getSubject();
			System.out.println(user.getUserName()+user.getPassword());
			UsernamePasswordToken token=new UsernamePasswordToken(user.getUserName(), CryptographyUtil.MD5(user.getPassword(), "test"));
			System.out.println(CryptographyUtil.MD5(user.getPassword(), "test"));
			try{
				subject.login(token);
				Session session=subject.getSession();
				System.out.println("sessionId:"+session.getId());
				System.out.println("sessionHost:"+session.getHost());
				System.out.println("sessionTimeout:"+session.getTimeout());
				session.setAttribute("user", user);
				
				return "redirect:/main.jsp";
			}catch(Exception e){
				e.printStackTrace();
				request.setAttribute("current", "用户名或密码错误！");
				return "login";
			}
		}
		
	}
