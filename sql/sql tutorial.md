## JDBC

Java Data Base Connectivity 的简称。由一系列链接，sql语句和和结果集构成

### 加载驱动

纯java驱动程序方式，由各数据库厂商提供提供各自的JDBC驱动程序，并且全部采用java语言编写

### JDBC API

#### DriverManager类
这是JDBC的驱动管理类，工作与数据库驱动程序与java应用程序之间

- getConnection(),试图与指定路径URL的数据库连接，属性prop中保存了数据库的用户名和密码
- URL的标注语法：jdbc:子协议://主机名:端口号，其中jdbc为协议，不同的厂商使用不同的子协议，比如mysql,不同数据库厂商使用的端口号不一样样**jdbc:mysql://localhost:3306**

#### Connection接口

- close()关闭数据库连接
- commit 向数据库提交事务
- createStatement() 创建一个Statement实例，以执行sql语句，并产生指定类型结果集
- isClose()
- getMetaData() 获取DatabaseMetaData 对象
- prepareStatement 创建一个PrepareStatement 对象执行带有参数的SQL

#### Statement接口

- 有三个不同的Statement接口，Statement，PrepareStatement，CallableStatement，分别为用于执行不带参数的简单SQL，用于执行预编译SQL语句，用于调用数据库的存储过程

#### PrepareStatement接口

- addBatch() 将一组参数添加到此PrepareStatement对象的批处理命令中
- excute() 在对象中，执行SQL语句，可以是任何种类的SQL语句
- excuteQuery() 并将查询结果以ResultSet的形式返回

#### ResultSet

### 建立连接

``` java
 	Class.forName("sun.jdbc.dobc.Jdcn0dbcDriver");
 	Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/news?user = root&password=passwd");

```

### 操作数据库
```java
	Statement statement = con.createment();//创建一个Statement实例来执行语句
	ResultSet rs = statement.executeQuerry("SELECT * FROM news");//执行查询语句,这是一个集合
```

### 对操作结果进行分析

```java
	while(rs.next()){	//遍历
	int id = rs.getInt('id');	//取值
	String title = fs.getString('title'); 	
	String username = rs.getString("username");
	Date time = rs.getDate("submitTime");

}
```

###关闭连接
```java
	rs.close();//关闭ResultSet实例
	statement.close();//关闭statement实例
	con.close() //关闭connection实例
```