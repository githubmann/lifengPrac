###Tomcat的配置与优化###
- 内存使用配置

###server.xml###
对应eclipse的tomcat配置选项

- wptwebapps eclipse默认部署目录
- webapps为tomcat默认部署目录


```xml

	<?xml version="1.0" encoding="UTF-8"?>
		<!-- port指定一个端口，负责监听关闭tomcat请求  -->
	  	<Server port="8005" shutdown="SHUTDOWN">
	  <!--APR library loader. Documentation at /docs/apr.html -->
	  	<Listener SSLEngine="on" className="org.apache.catalina.core.AprLifecycleListener"/>
	  <!--Initialize Jasper prior to webapps are loaded. Documentation at /docs/jasper-howto.html -->
	  	<Listener className="org.apache.catalina.core.JasperListener"/>
	  <!-- Prevent memory leaks due to use of particular java/javax APIs-->
	  	<Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener"/>
	  <!-- JMX Support for the Tomcat server. Documentation at /docs/non-existent.html -->
	  	<Listener className="org.apache.catalina.mbeans.ServerLifecycleListener"/>
	  	<Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener"/>

	  	<!-- 全局作用域资源 -->
	  	<GlobalNamingResources>
	   
	    	<Resource auth="Container" description="User database that can be updated and saved" 
	    		factory="org.apache.catalina.users.MemoryUserDatabaseFactory" name="UserDatabase" 
	    		pathname="conf/tomcat-users.xml" type="org.apache.catalina.UserDatabase"/>

	  	</GlobalNamingResources>
	
		<!-- 指定service的名字 -->
	  	<Service name="Catalina">
	  		<!-- defaultHost默认主机名-->
	  			
			<Engine defaultHost="localhost" name="Catalina">
			<!-- Connector表示客户端和service的连接 -->
			<!-- 监听来自port这个端口的请求，minProcessors:服务器启动时
			创建的处理请求的线程数。maxProcessors:为最大可创建的线程
			数， -->
			<!-- connectionTimeout：连接延迟 -->
			<!-- acceptCount:指定当所有可以使用的处理请求的线程数都被使用时，
			可以放到处理队列中的请求数，超过这个数的请求将不予处理-->
			 
		  	<Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" 
		    redirectPort="8443"/>
				
				<!-- 表示存放用户名,密码和role的数据库 -->
		 		<Realm className="org.apache.catalina.realm.UserDatabaseRealm" resourceName="UserDatabase"/>
				

				<!-- name:指定主机名
				webapps:应用程序基本目录，即存放应用程序的目录
				unpackWARS:如果为true ，则tomcat 会自动将WAR 文件解压，否则不解压
				，直接从WAR 文件中运行应用程序 -->
		  		<Host appBase="webapps" autoDeploy="true" name="localhost" unpackWARs="true" 
		      		xmlNamespaceAware="false" xmlValidation="false">
					
					<!-- docBase:应用程序的路径或者WAR文件存放的路径 -->
					<!-- path:表示此web应用程序url的前缀 -->

					<!-- reloadable:自动检测/WBE_INF/lib和/WEB_INF/classes,并加载 -->

			    	<Context docBase="D:\apache-tomcat-6.0.45\wtpwebapps\com.lifeng.servlet.test" 
			      		path="/test" reloadable="true" 
			      		source="org.eclipse.jst.jee.server:com.lifeng.servlet.test"/>

		    	</Host>
		    </Engine>
	  </Service>
	</Server>
````

###server.xml结构图###
![图片](../img/tomcat_server_xml.gif)



###tomcat-users.xml简析###


```xml
	<?xml version="1.0" encoding="UTF-8"?>

	<tomcat-users>


	  <role rolename="tomcat"/>
	  <role rolename="role1"/>
	  <user username="tomcat" password="tomcat" roles="tomcat"/>
	  <user username="both" password="tomcat" roles="tomcat,role1"/>
	  <user username="role1" password="tomcat" roles="role1"/>

	</tomcat-users>
```