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
                    org.springframework.we.web.context.ContextLoaderListener
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
- @controller使这个函数具有controller的功能，一个重要的事件是在 Spring MVC 的配置文件中通过 HandlerMapping 定义请求和控制器的映射关系，以便将两者关联起来。
- @requestMapping(value="baseType.do",method=RequestMethod.GET)[参考资料](http://www.cnblogs.com/qq78292959/p/3760560.html)
- @Responsebody表示该方法的返回结果直接写入HTTP response body中
一般在异步获取数据时使用，在使用@RequestMapping后，返回值通常解析为跳转路径，
加上@Responsebody后返回结果不会被解析为跳转路径，而是直接写入HTTP response body中。
比如异步获取json数据，加上@ResponseBody后，会直接返回json数据。
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
- 为什么？String n = "中国"; System.out.println(n);
输出的结果是：中国
我想问的是 n既然是String类的一个对象 是一个引用型数据 输出的为什么不是哈希码 怎么就直接输出堆内存中的内容了
例如 class A{}; A a = new A();System.out.println(a);
输出是：AA@41413EF???
```java
    // http://localhost:8080/array.do?name=Tom&name=Lucy&name=Jime
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



- 定义一个User,它有get,set方法,重写toString()方法
```java
    @override
    public String toString(){
        return "User("+
                    "name'"+name+'\''+
                    ", age=" +age +
                    ')';
    }
    // http://localhost:8080/object.do?name=Tom&age=24
    // 只要指定User对象的key即可
    @RequestMapping(value='object.do')
    @ResponseBody
    public String array(User user){
        return user.toString()
    }

```
- 多层级对象: http://localhost:8080/object.do?name=Tom&age=24&contactInfo.phone=10086&contactInfo=address=sofj
- 同属性对象
```java
    // http://localhost:8080/object.do?name=Tom&age=24
    // 只要指定User对象的key即可
    // User有name和age,admin里面有name和age   
    @RequestMapping(value='object.do')
    @ResponseBody
    public String array(User user，Admin admin){
        return user.toString()+admin.toString();
    }
    //利用WebDataBinder解决属性名冲突,InitBinder作用域是当前这个类
    @InitBinder("user")
    public void initUser(WebDataBinder binder){
        binder.setFiledDefaultPrefix('user.');
    }
        //同理
    @InitBinder("admin")
    public void initUser(WebDataBinder binder){
        binder.setFiledDefaultPrefix('admin.');
    }

```

####Unit5####
(参考资料)[http://www.cnblogs.com/HD/p/4107674.html]

- List
```java
    //这种写法无效,因为必须要将userlist封装，数据绑定是绑定其二级属性的
    @RequestMapping(value='list.do')
    @ResponseBody
    public String list(List<User> userList){
        return userList.toString();
    }
    // http://localhost:8080/list.do?users[0].name=Tom&users[1]].name=Lucy
    @RequestMapping(value='list.do')
    @ResponseBody
    public String list(UserListForm userListForm){
        return userListForm.toString();
    }

    //创建一个UserListForm类,里面有一个List<User> userList
```
- Set
- Map
-  http://localhost:8080/list.do?users[0].name=Tom&users[20].name=Lucy，用set将会报错，因为set不允许有空的元素
- 当我们在地址栏输入如：http://localhost:9080/es-web/vote?votes[0].voteItems[0].content=123时，会报：
org.springframework.beans.InvalidPropertyException: Invalid property 'voteItems[0]' of bean class [com.sishuok.es.test.Vote]: Cannot get element with index 0 from Set of size 0, accessed using property path 'voteItems[0]',***原因***：原因很明显，Set是无序列表，所以我们使用有顺序注入是不太合适的，**解决方案**:只要保证在springmvc绑定数据之前，给Set里边加上数据即可：
```java
    @ModelAttribute("votes")  
    public Set<Vote> initVotes() {  
        Set<Vote> votes = new HashSet<Vote>();  
        Vote vote = new Vote();  
        votes.add(vote);  
      
        vote.setVoteItems(new HashSet<VoteItem>());  
        vote.getVoteItems().add(new VoteItem());  
      
        return votes;  
    }  
```
- 绑定Set数据时，必须先在Set对象中add相应的数量的模型对象。
- (为什么要重写hashcode和equals)[../java/java basic]
- map的数据绑定(参考资料)[http://www.cnblogs.com/HD/p/4107674.html]




####Unit6####

- json
- xml
- json
```java
    //@RequestBody将{}
    @RequestMapping(value = "person/login")
    @ResponseBody
    public Person login(@RequestBody Person person) {//将请求中的mydata写入Person对象中
        return person;//不会被解析为跳转路径，而是直接写入HTTP response body中
    }
```
- [xml参考资料](http://www.imooc.com/video/11435)
####Unit7####
- PropetyEditor
- Formatter
- Converter

####Unit8####
- RESTful