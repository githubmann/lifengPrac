###java命名规范###
- Dao层
	- 接口类：实体对象+Interface+Dao,like:UserInterfaceDao，这就表明了这是Dao层的user接口
	- 实现类：实体对象+Interface+Impl+Dao,like:UserInterfaceImplDao 这表明了这是Dao层的user接口实现，定义各种字段和get,set方法
- Service层
	- 接口类：实体对象+Interface+Sevice,like:UserMsgInterfaceSevice，这就表明了这是Service层的user接口，也就是dao层user的延伸，增删查改的接口都在这里
		- dao层到service层的流动，like,**dao层接口类**(可写可不写)---->**dao层实现类**(定义字段和get,set方法)---->**service层接口类**(这里能就是定义service的接口，也就是增删查改的接口，like)--->**service层实现类(增删查改的实现,like:在这里面需要表明@Service，判空，执行sql语句，并返回值,其实这里就是增删查改的逻辑，而将语句的编写放进/resources/sqls/ProductMapper.xml)
		```java

		```
	- 实现类：实体对象+Interface+Impli+Sevice,like:UserMsgInterfaceImplService，这就表明了这是Service层的user接口实现类，也就是dao层的user的逻辑实现
