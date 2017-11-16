## Mybatis总结 ##
### 一、Mybatis简介 ###
	MyBatis是一个支持普通SQL查询，存储过程和高级映射的优秀持久层框架。MyBatis消除了几乎所有的JDBC代码和参数的手工设置以及对结果集的检索封装。MyBatis可
		以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数据库中的记录。
### 二、Mybatis好处 ###
	和JDBC比较： mybatis抽离出数据库的链接，关闭操作，抽离sql语句，并且可以自动的进行参数的设置，封装结果集。
	和Hibernate比较：
		-性能： Mybatis较Hibernate高
		-sql灵活性： Mybatis较Hibernate高
		-配置文件： Mybatis较Hibernate多 维护困难
		-数据库无关性： Mybatis较Hibernate低
### 三、Mybatis运行过程 ###
![](https://i.imgur.com/F67HsYc.jpg)

	 MyBatis应用程序根据XML配置文件创建SqlSessionFactory，SqlSessionFactory在根据配置，配置来源于两个地方，一处是配置文件，一处是Java代码的注解，获取
		一个SqlSession。SqlSession包含了执行sql所需要的所有方法，可以通过SqlSession实例直接运行映射的sql语句，完成对数据的增删改查和事务提交等，用完之
		后关闭SqlSession。
### 四、其它总结 ###
**处理较大文本时，数据库定义类型为longtext**	
**上传图片可以转换成字符再上传，及数据库类型为longblod**
**查询分页：**

	物理分页：定义map 的key value （start，。。） （size，。。）
		<select id="findStudent2"  resultMap="StudentResult" parameterType="Map">
			select * from t_student
			<if test="start!=null and size!=null">
				limit #{start},#{size}
			</if>
		</select>
	逻辑分页：定义RowBounds类    传入即可          RowBounds  rowBounds = new RowBounds(1, 3);
		<select id="findStudent"  resultMap="StudentResult">
			select * from t_student
		</select>
**关于缓存**

	Mybatis默认情况下，启用一级缓存，及同一个SqlSession接口对象调用相同select语句，则直接会从缓存中返回结果，而不是在查询一次数据库。

#### 四、支持逆向工程 ####
![](https://i.imgur.com/IkxzkOn.jpg)

**配置文件**

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE generatorConfiguration
	  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
	  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
	
	<generatorConfiguration>
		<context id="caigouTables" targetRuntime="MyBatis3">
			<commentGenerator>
				<!-- 是否去除自动生成的注释 true：是 ： false:否 -->
				<property name="suppressAllComments" value="true" />
			</commentGenerator>
			<!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
			<jdbcConnection driverClass="com.mysql.jdbc.Driver"
				connectionURL="jdbc:mysql://localhost:3306/uni" userId="root"
				password="mysql">
			</jdbcConnection> 
			<!-- <jdbcConnection driverClass="oracle.jdbc.OracleDriver"
				connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:yycg" 
				userId="yycg"
				password="yycg">
			</jdbcConnection>-->//orcale支持
	
			<!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer true，把JDBC DECIMAL 和 
				NUMERIC 类型解析为java.math.BigDecimal -->
			<javaTypeResolver>
				<property name="forceBigDecimals" value="false" />
			</javaTypeResolver>
	
			<!-- targetProject:自动生成代码的位置 -->
			<javaModelGenerator targetPackage="yycg.business.pojo.po"
				targetProject=".\src">
				<!-- enableSubPackages:是否让schema作为包的后缀 -->
				<property name="enableSubPackages" value="false" />
				<!-- 从数据库返回的值被清理前后的空格 -->
				<property name="trimStrings" value="true" />
			</javaModelGenerator>
	       <!-- targetPackage:mapper映射文件生成的位置 -->
			<sqlMapGenerator targetPackage="yycg.business.dao.mapper" 
				targetProject=".\src">
				<property name="enableSubPackages" value="false" />
			</sqlMapGenerator>
	       <!-- targetPackage：mapper接口的生成位置 -->
			<javaClientGenerator type="XMLMAPPER"
				targetPackage="yycg.business.dao.mapper" 
				targetProject=".\src">
				<property name="enableSubPackages" value="false" />
			</javaClientGenerator>
			
		
			<table schema="" tableName="userjd" /> //直接生成文件
		
			 
		
			  <table schema="" tableName="yybusiness" > //指定生成的变量类型
			  
			   <columnOverride column="zbjg" javaType="java.lang.Float" />
			   <columnOverride column="jyjg" javaType="java.lang.Float" />
			   <columnOverride column="cgl" javaType="java.lang.Integer" />
			   <columnOverride column="cgje" javaType="java.lang.Float" />
			  
			   <columnOverride column="rkje" javaType="java.lang.Float" />
			   <columnOverride column="ypyxq" javaType="java.lang.Float" />
			   <columnOverride column="rkl" javaType="java.lang.Integer" />
			   
			    <columnOverride column="thje" javaType="java.lang.Float" />
			    <columnOverride column="thl" javaType="java.lang.Integer" />
			    
			    <columnOverride column="jsje" javaType="java.lang.Float" />
			    <columnOverride column="jsl" javaType="java.lang.Integer" />
			    
			</table>
	
		</context>
	</generatorConfiguration>
	
**java类启动程序**

	public class GeneratorSqlmap {
	
		public void generator() throws Exception{
	
			List<String> warnings = new ArrayList<String>();
			boolean overwrite = true;
			//引入配置文件
			File configFile = new File("generatorConfig.xml"); 
			ConfigurationParser cp = new ConfigurationParser(warnings);
			Configuration config = cp.parseConfiguration(configFile);
			DefaultShellCallback callback = new DefaultShellCallback(overwrite);
			MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
					callback, warnings);
			myBatisGenerator.generate(null);
	
		} 
		public static void main(String[] args) throws Exception {
			try {
				GeneratorSqlmap generatorSqlmap = new GeneratorSqlmap();
				generatorSqlmap.generator();
			} catch (Exception e) {
				e.printStackTrace();
			}
			
		}
	
	}

**导入jar包**

		mybatis-generator-core-1.3.2.jar......

### 五、代码配置 ###


	  
	  