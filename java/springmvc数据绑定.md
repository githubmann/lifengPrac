####什么是数据绑定####
- 简单绑定是将一个用户界面元素（控件）的属性绑定到一个类型（对象）实例上的某个属性的方法。例如，如果一个开发者有一个Customer类型的实例，那么他就可以把Customer的“Name”属性绑定到一个TextBox的“Text”属性上。


####Unit2####
- 在IDEA选择maven-archetyp-webapp(maven webapp经典)这个结构
```java
        //目录结果
        - src
            -main
                -java
                -resource
                    -application.xml
                -webapp
                    -WEB_INF   
        
```
- (web.xml会加载spring容器)[http://blog.csdn.net/dddddz/article/details/8777126], 配置DispatcherServlet,filter.CharacterEncodingFilters 
```java
        <listener>
                <listener-class>
                    org.springframework.web.context.ContextLoaderListener
                </listener-class>
            </listener> 
            <!-- <!--contextConfigLocation在 ContextLoaderListener类中的默认值是 /WEB-INF/applicationContext.xml--> -->
            <context-param>
                <param-name>contextConfigLocation</param-name>
                <param-value>/WEB-INF/applicationContext.xml</param-value>
                <!-- <param-value>classpath:applicationContext*.xml</param-value> -->
            </context-param>
```
- applicationContext.xml配置包扫描
####Unit3####
年龄的数据绑定，究竟用int还是Integer(包装类型)
- 基本类型
- 包装类型
- 数组
- 如果定义的是基本类型，传参的时候不能为空，而如果是包装类型(Integer)可以为空,@RequestParam规定参数是否可以配置.**@RequestParam(value="xnumber", required=false) String number **:你从前端传过来的值为xnumber,如果为number将无法执行
```java
//你可通过定制的PropertyEditors 来扩展它的范围
// 这里@RequestParam注解可以用来提取名为“number”的String类型的参数，并将之作为输入参数传入。 
@RequestParam(value="number", required=false) String number **:你从前端传过来的值为
@RequestParam("id") Long id 
@RequestParam("balance") double balance 
@RequestParam double amount
```
- @controller是这个函数具有controller的功能，一个重要的事件是在 Spring MVC 的配置文件中通过 HandlerMapping 定义请求和控制器的映射关系，以便将两者关联起来。
- @requestMapping(value="baseType.do",method=RequestMethod.GET)[参考资料](http://www.cnblogs.com/qq78292959/p/3760560.html)
- @Responsebody表示该方法的返回结果直接写入HTTP response body中
一般在异步获取数据时使用，在使用@RequestMapping后，返回值通常解析为跳转路径，
加上@Responsebody后返回结果不会被解析为跳转路径，而是直接写入HTTP response body中。
比如异步获取json数据，加上@Responsebody后，会直接返回json数据。
@RequestBody将HTTP请求正文插入方法中,使用适合的HttpMessageConverter将请求体写入某个对象
```java
    function login() {//页面异步请求
        var mydata = '{"name":"' + $('#name').val() + '","id":"'
                + $('#id').val() + '","status":"' + $('#status').val() + '"}';
        $.ajax({
            type : 'POST',
            contentType : 'application/json',
            url : "${pageContext.request.contextPath}/person/login",
            processData : false,
            dataType : 'json',
            data : mydata,
            success : function(data) {
                alert("id: " + data.id + "\nname: " + data.name + "\nstatus: "
                        + data.status);
            },
            error : function() {
                alert('出错了！');
            }
        });
    };
        @RequestMapping(value = "person/login")
        @ResponseBody
        public Person login(@RequestBody Person person) {//将请求中的mydata写入Person对象中
            return person;//不会被解析为跳转路径，而是直接写入HTTP response body中
        }
````
- 将String对象数组快速输出(利用StringBuilder这个缓存区)
```java

```
- 为什么？String n = "中国"; System.out.println(n);
输出的结果是：中国
我想问的是 n既然是String类的一个对象 是一个引用型数据 输出的为什么不是哈希码 怎么就直接输出堆内存中的内容了
例如 class A{}; A a = new A();System.out.println(a);
输出是：AA@41413EF???
```java
    // http://localhost:8080/arry.do?name=Tom&name=Lucy&name=Jime
    // 这几相同key的参数将会被放入name数组中
    @RequestMapping(value='array.do')
    @ResponseBody
    public String array(String[] name){
        StringBuilder sbf = new StringBuilder();
        for(String item : name){
            sbf.append(item.append(" "))
        }
        return sbf.toString();
    }
```

####Unit4####
- 简单对象
- 多层级对象
- 同属性对象

####Unit5####
- LIst
- Set
- Map

####Unit6####

- json
- xml

####Unit7####
- PropetyEditor
- Formatter
- Converter

####Unit8####
- RESTful