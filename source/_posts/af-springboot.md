---
title: 微服务架构之 SpringBoot
date: 2019-04-04 18:17:48
categories:
- 应用框架
tags:
- 分布式
---

# 微服务架构之 SpringBoot

## 目录

- [简介](#简介)
- [快速入门](#快速入门)
- [正篇](#正篇)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

### `什么是SpringBoot`

```
SpringBoot是一个快速开发的框架,能够快速整合第三方框架,简化XML配置完全采用注解形式,内置Web服务器(Jetty和Tomcat),最终以java应用程序进行执行。
SpringBoot不是微服务框架。
```

### `为什么要用SpringBoot`

```
解决传统SSM项目的开发效率低、Jar冲突、配置多的问题。
```

### `与SpringMVC、SpringCloud之间的关系`

```
SpringCloud：
    是一套完整的微服务解决框架,采用Http+Json格式的通讯协议,底层依赖于SpringBoot框架(web组件集成springmvc),使用SpringMVC书写Http协议接口。
    采用SpringBoot+Dubbo也能实现微服务。
SpringMVC：
    SpringBoot默认集成了SpringMVC作为web层的框架。
总结：
    如果需要做微服务的话,就需要整合SpringBoot+SpringCloud;如果项目中不需要实现微服务,可以只单纯使用SpringBoot而不使用SpringCloud。
```

## 快速入门

1. 创建一个普通 maven 工程,packaging类型为jar(可省略)
2. pom文件配置
    ```
    <!-- 引入SpringBoot parent依赖 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.0.RELEASE</version>
    </parent>
    <!-- 设置资源属性 -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>
    <!-- 引入dependency -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>
    ```
    ```
    spring-boot-starter-parent作用
        统一整合第三方框架依赖信息,以后再申明其它dependency的时候就不需要写版本号了。
    spring-boot-starter-web作用
        把传统方式的SpringMVC依赖的所有jar全部给下载下来,
    spring-boot-maven-plugin作用
        如果直接通过Main启动spring，那么以下plugin必须要添加，否则是无法启动的。如果使用 maven 的spring-boot:run的话是不需要此配置的。
    ```
3. 配置application.yml
    ```
    server:
      tomcat:
        accept-count: 1000
        max-connections: 2000
        max-threads: 300
        min-spare-threads: 50
        uri-encoding: UTF-8 # 指定tomcat的编码格式
        max-http-post-size: 100MB
        accesslog:
          enabled: true
      port: 8080 # 设定web访问端口号，也是http监听端口
      connection-timeout: 60000
      servlet:
        context-path: /monitor # 2.0之后的配置，应用的上下文路径，也可以称为项目路径，是构成url地址的一部分
      compression:
        enabled: true # 是否开启压缩，默认为false
      http2:
        enabled: true # 应用支持http2
    ```
4. 编写helloworld服务
    ```
    package top.lvzhiqiang.controller;
    @RestController
    @EnableAutoConfiguration
    public class IndexController {
    
        @RequestMapping("/index")
        public String index(){
            return "hello springboot!";
        }
    
        public static void main(String[] args) {
            SpringApplication.run(IndexController.class,args);
        }
    }
    ```
    ```
    @RestController的作用
        表示该Controller下所有的方法返回JSON格式,可以直接编写Restful接口;省略了在方法上加上@ResponseBody。
        如果项目中的类加上了此注解,说明这个项目可能是一个微服务项目,需要统一接口,提供服务接口。
    @EnableAutoConfiguration的作用
        表示让Spring Boot根据添加的jar依赖来对Spring框架进行自动配置。
        由于spring-boot-starter-web添加了Tomcat和Spring MVC，所以auto-configuration将假定你正在开发一个web应用并相应地对Spring进行设置。
    @Configuration的作用
        底层含有@Component,标注在类上,相当于把该类作为spring的xml配置文件中的<beans>
        用来来配置spring容器(应用上下文)
    @Bean的作用
        标注在方法上(返回某个实例的方法),相当于spring的xml配置文件中的<bean>
        用来注册bean对象(实例化bean,并交给spring管理)
    ```
5. 启动应用
    ```
    方法1：
        直接启动main方法,main方法中指定IndexController为启动入口,默认端口号为8080。
    方法2：
        @ComponentScan(basePackages = "top.lvzhiqiang.controller")
        @EnableAutoConfiguration
        public class App {
        	public static void main(String[] args) {
        		SpringApplication.run(App.class, args);
        	}
        }
        单独写个类并使用@ComponentScan注解添加自动扫包范围。
    方法3：
        @SpringBootApplication
        public class App {
            public static void main(String[] args) {
                SpringApplication.run(App.class, args);
            }
        }
        单独写个类并使用@SpringBootApplication注解,等同于@ComponentScan + @EnableAutoConfiguration。
        扫包范围：当前包下或者子包下所有的类都可以扫到。
    ```

## 正篇

### Web开发

- `静态资源访问`
    ```
    在我们开发Web应用的时候，需要引用大量的js、css、图片等静态资源。
    Spring Boot默认提供静态资源目录位置需置于classpath(src/main/java或者src/main/resources)下，目录命名如下：
        static、public、resources、META-INF/resources
    ```
- `渲染Web页面`
    ```
    如果需要返回html等页面时,Spring Boot提供了多种模板引擎,来帮助我们很快的上手开发动态网站。
        Thymeleaf、FreeMarker、Velocity、Groovy、Mustache
    Spring Boot建议使用模板引擎(例如FreeMarker)，避免使用JSP，若一定要使用JSP将无法实现Spring Boot的多种特性。
        如果使用模板引擎,它们默认的模板配置路径为：src/main/resources/templates。当然也可以修改这个路径。
    ```
- `使用Freemarker模板引擎渲染Web视图`
    ```
    1.pom文件引入FreeMarker的依赖包
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-freemarker</artifactId>
        </dependency>
    2.在src/main/resources/创建一个templates文件夹,在templates下新建ftlIndex.ftl
        @Controller
        @EnableAutoConfiguration
        public class IndexController {
            @RequestMapping("/ftlIndex")
            public String ftlIndex(Model model) {
                model.addAttribute("name", "张三");
                model.addAttribute("sex", 1);
                List<String> userList = new ArrayList<>();
                userList.add("a");
                userList.add("b");
                userList.add("c");
                model.addAttribute("userList", userList);
                return "ftlIndex";
            }
            ...
        }
    3.在ftl文件中填充数据
        ${name}
        <#if sex == 1>男
        <#elseif sex == 2>女
        <#else>其他
        </#if>
        <#list userList as user>${user}
        </#list>
    4.新建src/main/resources/下新建application.properties文件进行Freemarker配置(可以不配置,采用默认)
        spring.freemarker.allow-request-override=false
        spring.freemarker.cache=true
        spring.freemarker.check-template-location=true
        spring.freemarker.charset=UTF-8
        spring.freemarker.content-type=text/html
        spring.freemarker.expose-request-attributes=false
        spring.freemarker.expose-session-attributes=false
        spring.freemarker.expose-spring-macro-helpers=false
        #spring.freemarker.prefix=
        #spring.freemarker.request-context-attribute=
        #spring.freemarker.settings.*=
        spring.freemarker.suffix=.ftl
        spring.freemarker.template-loader-path=classpath:/templates/
        #comma-separated list
        #spring.freemarker.view-names= # whitelist of view names that can be resolved
    ```
- `使用JSP渲染Web视图`
    ```
    1.pom文件引入Jsp相关的依赖包
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
        </dependency>
    2.新建src/main/resources/下新建application.properties文件进行Jsp配置
        spring.mvc.view.prefix=/WEB-INF/jsp/
        spring.mvc.view.suffix=.jsp
    3.在src/main/webapp下创建/WEB-INF/jsp/目录,并在jsp目录下新建jspIndex.jsp
        @Controller
        @EnableAutoConfiguration
        public class IndexController {
            @RequestMapping("/jspIndex")
            public String jspIndex() {
                return "jspIndex";
            }
            ...
        }
    4.如果找不到页面,可在pom文件中改为war类型项目
    ```
- `全局捕获异常`
    ```
    @ControllerAdvice(basePackages = "top.lvzhiqiang.controller")
    public class GlobalExceptionHandler {
        @ExceptionHandler(RuntimeException.class)
        @ResponseBody
        public Map<String, Object> exceptionHandler() {
            Map<String, Object> map = new HashMap<>();
            map.put("errorCode", "101");
            map.put("errorMsg", "系統错误!");
            return map;
        }
    }
    ```
    ```
    @ExceptionHandler表示拦截异常
    @ControllerAdvice是controller的一个辅助类，最常用的就是作为全局异常处理的切面类
        @ControllerAdvice可以指定扫描范围
        @ControllerAdvice约定了几种可行的返回值，如果是直接
            返回json，需要使用@ResponseBody进行json转换
            返回页面，返回值类型为ModelAndView或者String
    实际开发中,可在全局捕获异常中将异常日志信息写入nosql数据库中,如果是集群环境,可采用分布式日志收集系统(kafka,logstash等),方便排查
    ```

### 日志管理

1. `使用log4j记录日志`
    ```
    1.pom文件引入log4j相关的依赖包
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <exclusions>
                <!-- 排除自带的logback依赖 -->
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-log4j</artifactId>
            <version>1.3.8.RELEASE</version>
        </dependency>
    2.新建src/main/resources/下新建log4j.properties文件进行log4j配置
        log4j.rootLogger=info,error,CONSOLE,DEBUG
        log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
        log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
        log4j.appender.CONSOLE.layout.ConversionPattern=%d{yyyy-MM-dd-HH-mm} [%t] [%c] [%p] - %m%n
        log4j.logger.info=info
        log4j.appender.info=org.apache.log4j.DailyRollingFileAppender
        log4j.appender.info.layout=org.apache.log4j.PatternLayout
        log4j.appender.info.layout.ConversionPattern=%d{yyyy-MM-dd-HH-mm} [%t] [%c] [%p] - %m%n
        log4j.appender.info.datePattern='.'yyyy-MM-dd
        log4j.appender.info.Threshold = info
        log4j.appender.info.append=true
        log4j.appender.info.File=D:\\log4j\\springbootdemo\\info
        log4j.logger.error=error
        log4j.appender.error=org.apache.log4j.DailyRollingFileAppender
        log4j.appender.error.layout=org.apache.log4j.PatternLayout
        log4j.appender.error.layout.ConversionPattern=%d{yyyy-MM-dd-HH-mm} [%t] [%c] [%p] - %m%n
        log4j.appender.error.datePattern='.'yyyy-MM-dd
        log4j.appender.error.Threshold = error
        log4j.appender.error.append=true
        log4j.appender.error.File=D:\\log4j\\springbootdemo\\error
        log4j.logger.DEBUG=DEBUG
        log4j.appender.DEBUG=org.apache.log4j.DailyRollingFileAppender
        log4j.appender.DEBUG.layout=org.apache.log4j.PatternLayout
        log4j.appender.DEBUG.layout.ConversionPattern=%d{yyyy-MM-dd-HH-mm} [%t] [%c] [%p] - %m%n
        log4j.appender.DEBUG.datePattern='.'yyyy-MM-dd
        log4j.appender.DEBUG.Threshold = DEBUG
        log4j.appender.DEBUG.append=true
        log4j.appender.DEBUG.File=D:\\log4j\\springbootdemo\\debug
    ```
2. `使用AOP统一处理Web请求日志`
    ```
    1.pom文件新增aop依赖
        <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
    2.aop代码
        @Aspect
        @Component
        public class WebLogAspect {
            private static final Logger logger = LoggerFactory.getLogger(WebLogAspect.class);
        
            @Pointcut("execution(public * top.lvzhiqiang.controller.*.*(..))")
            public void webLog() {
            }
        
            @Before("webLog()")
            public void doBefore(JoinPoint joinPoint) throws Throwable {
                // 接收到请求，记录请求内容
                ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
                HttpServletRequest request = attributes.getRequest();
                // 记录下请求内容
                logger.info("URL : " + request.getRequestURL().toString());
                logger.info("HTTP_METHOD : " + request.getMethod());
                logger.info("IP : " + request.getRemoteAddr());
                Enumeration<String> enu = request.getParameterNames();
                while (enu.hasMoreElements()) {
                    String name = (String) enu.nextElement();
                    logger.info("name:{},value:{}", name, request.getParameter(name));
                }
            }
        
            @AfterReturning(returning = "ret", pointcut = "webLog()")
            public void doAfterReturning(Object ret) throws Throwable {
                // 处理完请求，返回内容
                logger.info("RESPONSE : " + ret);
            }
        }
    ```
3. `Spring Boot集成lombok让代码更简洁`
    ```
    0.IDE开发工具需要先安装lombok插件(lombok底层使用asm字节码技术,在编译时修改字节码文件,来生成相应的set,get等相应方法)
    1.pom文件引入lombok相关的依赖包
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    2.实体类测试
        @Slf4j
        @Setter
        @Getter
        public class UserEntity {
            // @Getter
            // @Setter
            private String userName;
            // @Getter
            // @Setter
            private Integer age;
        
            @Override
            public String toString() {
                return "UserEntity [userName=" + userName + ", age=" + age + "]";
            }
        
            public static void main(String[] args) {
                UserEntity userEntity = new UserEntity();
                userEntity.setUserName("zhangsan");
                userEntity.setAge(20);
                System.out.println(userEntity.toString());
                log.info("####我是日志##########");
            }
        }
    ```
    ```
    其他特性：
        @Data: 自动生成set/get方法，toString方法，equals方法，hashCode方法，不带参数的构造方法
        @Setter/@Getter: 自动生成set和get方法 
        @ToString: 自动生成toString方法 
        @NonNull: 让你不在担忧并且爱上NullPointerException 
        @CleanUp: 自动资源管理,不用再在finally中添加资源的close方法 
        @EqualsAndHashcode: 从对象的字段中生成hashCode和equals的实现 
        @NoArgsConstructor/@RequiredArgsConstructor/@AllArgsConstructor: 自动生成构造方法
        @Value: 用于注解final类 
        @Builder: 产生复杂的构建器api类 
        @SneakyThrows: 异常处理（谨慎使用） 
        @Synchronized : 同步方法安全的转化 
        @Log: 支持各种logger对象，使用时用对应的注解，如：@Log4j
    ```
    
### 数据访问

- `整合mybatis和HikariCP`
    ```
    1.pom文件中引入数据源驱动与mybatis依赖
        <!-- mybatis -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.1.1</version>
        </dependency>
        <!-- mysql驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.21</version>
        </dependency>
    2.在application.yml中配置数据源和mybatis
        ###########################################################
        #
        # 配置数据源信息
        #
        ###########################################################
        spring:
          datasource: # 数据源的相关配置
            type: com.zaxxer.hikari.HikariDataSource # 数据源类型：HikariCP
            driver-class-name: com.mysql.jdbc.Driver # mysql驱动
            url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=UTF-8&autoReconnect
            username: root
            password: root
          hikari: # Hikari连接池配置
            connection-timeout: 30000 # 等待连接池分配连接的最大时长（毫秒），超过这个时长还没可用的连接则发生SQ
            minimum-idle: 5 # 最小空闲连接数
            maximum-pool-size: 20 # 最大连接数，默认是10
            auto-commit: true # 自动提交
            idle-timeout: 600000 # 连接超时的最大时长（毫秒），超时则被释放（retired），默认:10分钟
            pool-name: DateSourceHikariCP # 连接池名字
            max-lifetime: 1800000 # 此属性控制池中连接的最长生命周期（毫秒），值0表示无限生命周期，超时而且没被使用则被释放（retired），默认:30分钟
            connection-test-query: SELECT 1
        ###########################################################
        #
        # mybatis配置
        #
        ###########################################################
        mybatis:
          type-aliases-package: com.riskraider.monitor.entity # 所有POJO类所在包路径，只能指定具体的包，多个配置可以使用英文逗号隔开
          mapper-locations: classpath:mapper/**/*.xml # mapper映射文件的路径，支持Ant风格的通配符，多个配置可以使用英文逗号隔开
          configuration:
            # 开启驼峰命名转换，如：Table(create_time) -> Entity(createTime)。
            # 不需要我们关心怎么进行字段匹配，mybatis会自动识别`大写字母与下划线`
            map-underscore-to-camel-case: true
    3.Mapper代码
        public interface UserMapper {
        	@Select("SELECT * FROM USERS WHERE NAME = #{name}")
        	User findByName(@Param("name") String name);
        	@Insert("INSERT INTO USERS(NAME, AGE) VALUES(#{name}, #{age})")
        	int insert(@Param("name") String name, @Param("age") Integer age);
        }
    4.在启动类上加上@MapperScan(basePackages = "top.lvzhiqiang.mapper")
        也可以在Mapper接口上加上@Mapper,不过这样比较繁琐
    ```
    ```
    Mybatis整合分页插件(pageHelper)
        PageHelper是一款好用的开源免费的Mybatis第三方物理分页插件
        支持常见的12种数据库。Oracle,MySql,MariaDB,SQLite,DB2,PostgreSQL,SqlServer等
        支持多种分页方式
        支持常见的 RowBounds(PageRowBounds)，PageHelper.startPage方法调用，Mapper接口参数调用
    ------------------
    1.添加maven依赖
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.5</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.7</version>
        </dependency>
    2.添加配置
        pagehelper.helperDialect=mysql
        pagehelper.reasonable=true
        pagehelper.supportMethodsArguments=true
        pagehelper.params=count=countSql
        pagehelper.page-size-zero=true
    3.service层
        public PageInfo<User> findUserList(int pageNo, int pageSize) {
            // 开启分页插件,放在查询语句上面
            PageHelper.startPage(pageNo, pageSize);
            List<User> listUser = userMapper.findUserList();
            // 封装分页之后的数据
            PageInfo<User> pageInfoUser = new PageInfo<User>(listUser);
            return pageInfoUser;
        }
        //以上方法上的公共代码可用模板设计模式或者aop思想抽出来
    ```
- `整合jdbcTemplate`
    ```
    1.pom文件引入jdbcTemplate相关的依赖包
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.21</version>
        </dependency>
    2.修改application.properties
        spring.datasource.jdbc-url=jdbc:mysql://localhost:3306/test
        spring.datasource.username=root
        spring.datasource.password=root
        spring.datasource.driver-class-name=com.mysql.jdbc.Driver
    3.代码
        @Service
        public class UserServiceImpl implements UserService {
        	@Autowired
        	private JdbcTemplate jdbcTemplate;
        	public void createUser(String name, Integer age) {
        		jdbcTemplate.update("insert into users values(null,?,?);", name, age);
        	}
        }
    ```
- `整合多数据源`
    ```
    1.修改application.properties新增两个数据源
        ###datasource1
        spring.datasource.test1.driver-class-name = com.mysql.jdbc.Driver
        spring.datasource.test1.jdbc-url = jdbc:mysql://localhost:3306/test01?useUnicode=true&characterEncoding=utf-8
        spring.datasource.test1.username = root
        spring.datasource.test1.password = root
        ###datasource2
        spring.datasource.test2.driver-class-name = com.mysql.jdbc.Driver
        spring.datasource.test2.jdbc-url = jdbc:mysql://localhost:3306/test02?useUnicode=true&characterEncoding=utf-8
        spring.datasource.test2.username = root
        spring.datasource.test2.password = root
    2.在top/lvzhiqiang/下新建datasource目录(任意),新建DataSourceConfig类,采用分包(业务)机制来读取不同的数据源
        //datasource1
        @Configuration
        @MapperScan(basePackages = "top.lvzhiqiang.user1", sqlSessionFactoryRef = "test1SqlSessionFactory")
        public class DataSource1Config {
            @Bean(name = "test1DataSource")
            @Primary
            @ConfigurationProperties(prefix = "spring.datasource.test1")
            public DataSource testDataSource() {
                return DataSourceBuilder.create().build();
            }
            @Bean(name = "test1SqlSessionFactory")
            @Primary
            public SqlSessionFactory testSqlSessionFactory(@Qualifier("test1DataSource") DataSource dataSource)
                    throws Exception {
                SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
                bean.setDataSource(dataSource);
                //bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mybatis/mapper/test1/*.xml"));
                return bean.getObject();
            }
            @Bean(name = "test1TransactionManager")
            @Primary
            public DataSourceTransactionManager testTransactionManager(@Qualifier("test1DataSource") DataSource dataSource) {
                return new DataSourceTransactionManager(dataSource);
            }
        
            @Bean(name = "test1SqlSessionTemplate")
            public SqlSessionTemplate testSqlSessionTemplate(@Qualifier("test1SqlSessionFactory") SqlSessionFactory sqlSessionFactory)
                    throws Exception {
                return new SqlSessionTemplate(sqlSessionFactory);
            }
        }
        ------------------------------
        //datasource2
        @Configuration
        @MapperScan(basePackages = "top.lvzhiqiang.user2", sqlSessionFactoryRef = "test2SqlSessionFactory")
        public class DataSource2Config {
            @Bean(name = "test2DataSource")
            @ConfigurationProperties(prefix = "spring.datasource.test2")
            public DataSource testDataSource() {
                return DataSourceBuilder.create().build();
            }
            ...
        }
    3.创建分包Mapper
        public interface User1Mapper {
        	@Insert("insert into users values(null,#{name},#{age});")
        	public int addUser(@Param("name") String name, @Param("age") Integer age);
        }
    4.多数据源事务注意事项
        在多数据源的情况下，使用@Transactional注解时，应该指定事务管理器
        @Transactional(transactionManager = "test2TransactionManager")
    ```

### 事务管理

- `SpringBoot整合事物管理`
    ```
    springboot默认集成并开启事物,只要在方法上加上@Transactional即可
    ```
- `SpringBoot分布式事物管理`
    ```
    针对多数据源的分布式事务解决方案：jta+atomikos
    针对微服务的分布式事务解决方案：mq或者tcc等
    ```
    ```
    1.pom文件引入atomikos相关的依赖包
        <dependency>
        	<groupId>org.springframework.boot</groupId>
        	<artifactId>spring-boot-starter-jta-atomikos</artifactId>
        </dependency>
    2.新增多数据源配置信息,修改application.properties
        # Mysql 1
        mysql.datasource.test1.url = jdbc:mysql://localhost:3306/test01?useUnicode=true&characterEncoding=utf-8
        mysql.datasource.test1.username = root
        mysql.datasource.test1.password = root
        mysql.datasource.test1.minPoolSize = 3
        mysql.datasource.test1.maxPoolSize = 25
        mysql.datasource.test1.maxLifetime = 20000
        mysql.datasource.test1.borrowConnectionTimeout = 30
        mysql.datasource.test1.loginTimeout = 30
        mysql.datasource.test1.maintenanceInterval = 60
        mysql.datasource.test1.maxIdleTime = 60
        # Mysql 2
        mysql.datasource.test2.url =jdbc:mysql://localhost:3306/test02?useUnicode=true&characterEncoding=utf-8
        mysql.datasource.test2.username =root
        mysql.datasource.test2.password =root
        mysql.datasource.test2.minPoolSize = 3
        mysql.datasource.test2.maxPoolSize = 25
        mysql.datasource.test2.maxLifetime = 20000
        mysql.datasource.test2.borrowConnectionTimeout = 30
        mysql.datasource.test2.loginTimeout = 30
        mysql.datasource.test2.maintenanceInterval = 60
        mysql.datasource.test2.maxIdleTime = 60
    3.读取多数据源配置信息,可在top/lvzhiqiang/下新建config目录(任意),新建2个config类
        @Data
        @ConfigurationProperties(prefix = "mysql.datasource.test1")
        public class DBConfig1 {
        	private String url;
        	private String username;
        	private String password;
        	private int minPoolSize;
        	private int maxPoolSize;
        	private int maxLifetime;
        	private int borrowConnectionTimeout;
        	private int loginTimeout;
        	private int maintenanceInterval;
        	private int maxIdleTime;
        	private String testQuery;
        }
        ----------------------
        @Data
        @ConfigurationProperties(prefix = "mysql.datasource.test2")
        public class DBConfig2 {
        	private String url;
        	private String username;
        	private String password;
        	private int minPoolSize;
        	private int maxPoolSize;
        	private int maxLifetime;
        	private int borrowConnectionTimeout;
        	private int loginTimeout;
        	private int maintenanceInterval;
        	private int maxIdleTime;
        	private String testQuery;
        }
    4.将多数据源注册到atomikos中去,在top/lvzhiqiang/下新建datasource目录(任意),新建DataSourceConfig类
        @Configuration
        @MapperScan(basePackages = "top.lvzhiqiang.test01", sqlSessionTemplateRef = "testSqlSessionTemplate")
        public class MyBatisConfig1 {
        	@Primary
        	@Bean(name = "testDataSource")
        	public DataSource testDataSource(DBConfig1 testConfig) throws SQLException {
        		MysqlXADataSource mysqlXaDataSource = new MysqlXADataSource();
        		mysqlXaDataSource.setUrl(testConfig.getUrl());
        		mysqlXaDataSource.setPinGlobalTxToPhysicalConnection(true);
        		mysqlXaDataSource.setPassword(testConfig.getPassword());
        		mysqlXaDataSource.setUser(testConfig.getUsername());
        		mysqlXaDataSource.setPinGlobalTxToPhysicalConnection(true);
        
        		AtomikosDataSourceBean xaDataSource = new AtomikosDataSourceBean();
        		xaDataSource.setXaDataSource(mysqlXaDataSource);
        		xaDataSource.setUniqueResourceName("testDataSource");
        
        		xaDataSource.setMinPoolSize(testConfig.getMinPoolSize());
        		xaDataSource.setMaxPoolSize(testConfig.getMaxPoolSize());
        		xaDataSource.setMaxLifetime(testConfig.getMaxLifetime());
        		xaDataSource.setBorrowConnectionTimeout(testConfig.getBorrowConnectionTimeout());
        		xaDataSource.setLoginTimeout(testConfig.getLoginTimeout());
        		xaDataSource.setMaintenanceInterval(testConfig.getMaintenanceInterval());
        		xaDataSource.setMaxIdleTime(testConfig.getMaxIdleTime());
        		xaDataSource.setTestQuery(testConfig.getTestQuery());
        		return xaDataSource;
        	}
        	@Primary
        	@Bean(name = "testSqlSessionFactory")
        	public SqlSessionFactory testSqlSessionFactory(@Qualifier("testDataSource") DataSource dataSource)
        			throws Exception {
        		SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        		bean.setDataSource(dataSource);
        		return bean.getObject();
        	}
        	@Primary
        	@Bean(name = "testSqlSessionTemplate")
        	public SqlSessionTemplate testSqlSessionTemplate(@Qualifier("testSqlSessionFactory") SqlSessionFactory sqlSessionFactory)
        	        throws Exception {
        		return new SqlSessionTemplate(sqlSessionFactory);
        	}
        }
        ----------------------
        @Configuration
        @MapperScan(basePackages = "top.lvzhiqiang.test02", sqlSessionTemplateRef = "test2SqlSessionTemplate")
        public class MyBatisConfig2 {
        	@Bean(name = "test2DataSource")
        	public DataSource testDataSource(DBConfig2 testConfig) throws SQLException {
        		MysqlXADataSource mysqlXaDataSource = new MysqlXADataSource();
        		mysqlXaDataSource.setUrl(testConfig.getUrl());
        		mysqlXaDataSource.setPinGlobalTxToPhysicalConnection(true);
        		...
        	}
            ...
        }
    5.在启动类加上此注解以开启读取配置文件
        @EnableConfigurationProperties(value = { DBConfig1.class, DBConfig2.class })
    ```

### 监控管理

- `Actuator监控应用`
    ```
    什么是SpringBoot监控中心
        Actuator是spring boot的一个附加功能,可帮助你在应用程序生产环境时监视和管理应用程序。
        特别对于微服务管理十分有意义,缺点是没有可视化界面(返回json格式)。
        默认情况下,监控中心只提供3个接口权限。
        在SpringBoot2.0后,监控中心接口地址发生变化,需要在访问接口前面中上/actuator。
    主要用途
        针对微服务服务器监控,包括服务器的内存变化(堆内存、线程、日志管理等)
        检测服务配置连接地址(懒加载形式)是否可用(模拟访问)
        统计当前Spring容器中有多少个bean
        统计SpringMVC有多少个@RuquestMapping(http接口)
    应用场景
        生产环境
    ```
    ```
    1.添加maven依赖
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
    2.yml配置
        ###通过下面的配置启用所有的监控端点，默认情况下，这些端点是禁用的
        management:
            endpoints:
                web:
                    exposure:
                        include: "*"
    3.开始访问(通过actuator/+端点名就可以获取相应的信息)
        /actuator/beans	显示应用程序中所有Spring bean的完整列表。
        /actuator/mappings	显示所有@RequestMapping的url完整列表。
        /actuator/configprops	显示所有配置信息。
        /actuator/env	显示所有的环境变量。
        /actuator/health	显示应用程序运行状况信息,up表示成功,down失败。
        /actuator/info	查看自定义应用信息(显示在配置文件配置info开头的配置信息)     
    ```
- `Admin-UI分布式微服务监控中心`
    - Admin-UI底层基于actuator实现能够返回界面展示监控信息,原理是将所有服务的监控中心管理存放在admin-ui平台上。
    ---
    ```
    Admin-UI-Server
    --------------------
    1.添加maven依赖
        <dependency>
            <groupId>de.codecentric</groupId>
            <artifactId>spring-boot-admin-starter-server</artifactId>
            <version>2.0.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-webflux</artifactId>
        </dependency>
        <dependency>
            <groupId>org.jolokia</groupId>
            <artifactId>jolokia-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>com.googlecode.json-simple</groupId>
            <artifactId>json-simple</artifactId>
            <version>1.1</version>
        </dependency>
    2.yml配置
        spring:
            application:
                name: spring-boot-admin-server
    3.启动server端
        @Configuration
        @EnableAutoConfiguration
        @EnableAdminServer
        public class AdminServerApplication {
        	public static void main(String[] args) {
        		SpringApplication.run(AdminServerApplication.class, args);
        	}
        }
    4.访问
        http://localhost:8080
    ```
    ```
    Admin-UI-Client
    --------------------
    1.添加maven依赖
        <dependency>
            <groupId>de.codecentric</groupId>
            <artifactId>spring-boot-admin-starter-client</artifactId>
            <version>2.0.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.jolokia</groupId>
            <artifactId>jolokia-core</artifactId>
        </dependency>
        <dependency>
            <groupId>com.googlecode.json-simple</groupId>
            <artifactId>json-simple</artifactId>
            <version>1.1</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    2.yml配置
        spring:
            boot:
                admin:
                    client:
                        url: http://localhost:8080    ###配置client端注册到admin-ui平台
        server:
            port: 8081
        management:
            endpoints:
                web:
                    exposure:
                        include: "*"
                endpoint:
                    health:
                        show-details: ALWAYS
    3.启动client端
        @SpringBootApplication
        public class AppClient {
        	public static void main(String[] args) {
        		SpringApplication.run(AppClient.class, args);
        	}
        }
    ```

### 性能优化

- `JVM参数调优(运行优化)`
    ```
    -Xms和-Xmx最好设置成一样,不然会频繁造成GC
    外部运行调优使用：java -server -Xms32m -Xmx32m  -jar springboot.jar
    ```
- `扫包优化(启动优化,非运行优化)`
    ```
    针对大项目时不要用@SpringBootApplication注解,会扫描到无用的包,可以采用@ComponentScan来扫描指定的包
    ```
- `将Servlet容器变成Undertow`
    ```
    默认情况下，Spring Boot 使用 Tomcat 来作为内嵌的 Servlet 容器,可以将 Web 服务器切换到 Undertow 来提高应用性能。
        Undertow 是一个采用 Java 开发的灵活的高性能 Web 服务器，提供包括阻塞和基于NIO的非堵塞机制。
        Undertow 是红帽公司的开源产品，是 Wildfly 默认的 Web 服务器。
    可用jmeter作吞吐量测试,结果是tomcat吞吐量为5000,undertow为8000
    ```
    ```
    1.从依赖信息里移除Tomcat
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    2.添加Undertow依赖
        <dependency>
        	<groupId>org.springframework.boot</groupId>
        	<artifactId>spring-boot-starter-undertow</artifactId>
        </dependency>
    ```

### 其他内容

- `使用@Scheduled创建定时任务`
    ```
    在启动类中加入@EnableScheduling注解，启用定时任务的配置
    -------------------
    @Component
    public class ScheduledTasks {
        private static final SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");
        @Scheduled(fixedRate = 5000)
        public void reportCurrentTime() {
            System.out.println("现在时间：" + dateFormat.format(new Date()));
        }
    }
    ```
- `使用@Async异步执行方法`
    ```
    在启动类加上@EnableAsync来开启异步调用,并在需要执行异步的方法上加上@Async
        @Async注解使用AOP技术在运行时创建一个单线程来执行这个方法
    ```
- `使用@Value自定义参数`
    ```
    application.properties文件
        name=lvzhiqiang.top
    代码中
    	@Value("${name}")
    	private String name;
        @ResponseBody
    	@RequestMapping("/getValue")
    	public String getValue() {
    		return name;
    	}
    ```
- `区分不同环境配置文件`
    ```
    1.在src/main/resources下新建各个环境的配置文件
        application-dev.properties：开发环境
        application-uat.properties：测试环境
        application-pre.properties：生产环境
    2.在application.properties中进行切换
        spring.profiles.active=uat
    ```
- `修改端口号`
    ```
    server.port=8888
    server.context-path=/springbootdemo
    ```
- `Spring Boot yml 使用`
    ```
    SpringBoot默认读取application.yml|properties
    YML比properties配置文件更加简约（结构）
    ---------------
    server:
      port: 8081
      context-path: /springbootdemo
    ```
- `发布打包`
    ```
    jar类型打包方式
        1.使用mvn celan  package 打包
        2.使用java –jar 包名
    war类型打包方式
        1.使用mvn celan  package 打包
        2.将war包放入到tomcat webapps下运行即可
        注意:springboot2.0内置tomcat8.5.25，建议使用外部Tomcat9.0版本运行即可,否则报错版本不兼容。
    ```
    ```
    如果执行java -jar命令提示没有主清单属性时,可在pom文件中新增如下代码
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                    </configuration>
                </plugin>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <configuration>
                        <mainClass>top.lvzhiqiang.StartApp</mainClass>
                    </configuration>
                    <executions>
                        <execution>
                            <goals>
                                <goal>repackage</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    ```
- `热部署`
    ```
    1.什么是热部署
        在应用程序不停止的情况下，实现新的部署
    2.热部署原理
        使用类加载器重新读取字节码文件到JVM中去
    3.纯手写实现一个热部署功能
        监听class文件是否发生改变(版本号或者修改时间),如果class文件发生改变,就使用classloader进行得新读取
    4.优缺点
        适合本地开发,提高运行效率,不用重启服务器;缺点是项目比较大时,非常卡,比较占内存.
    ```
    ```
    1.引入devtools依赖
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
            <scope>true</scope>
        </dependency>
    2.Devtools原理
        1.devtools会监听classpath下的文件变动，并且会立即重启应用（发生在保存时机），注意：因为其采用的虚拟机机制，该项重启是很快的。  
        2.devtools可以实现页面热部署（即页面修改后会立即生效，这个可以直接在application.properties文件中配置spring.thymeleaf.cache=false来实现(这里注意不同的模板配置不一样) 
    3.不推荐使用
    ```
- `整合拦截器`
    ```
    1.创建拦截器
        @Slf4j
        @Component
        //创建模拟登录拦截器，验证请求是否有token参数
        public class LoginIntercept implements HandlerInterceptor {
            public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
                    throws Exception {
                log.info("开始拦截登录请求....");
                String token = request.getParameter("token");
                if (StringUtils.isEmpty(token)) {
                    response.getWriter().println("not found token");
                    return false;
                }
                return true;
            }
        }
    2.注册拦截器
        @Configuration
        public class WebAppConfig {
        	@Autowired
        	private LoginIntercept loginIntercept;
        	@Bean
        	public WebMvcConfigurer WebMvcConfigurer() {
        		return new WebMvcConfigurer() {
        			public void addInterceptors(InterceptorRegistry registry) {
        				registry.addInterceptor(loginIntercept).addPathPatterns("/*");
        			};
        		};
        	}
        }
    ```
    ```
    拦截器与过滤器区别
        拦截器是AOP( Aspect-Oriented Programming)的一种实现，底层通过动态代理模式完成。
        （1）拦截器是基于java的反射机制的，而过滤器是基于函数回调。
        （2）拦截器不依赖于servlet容器，而过滤器依赖于servlet容器。
        （3）拦截器只能对Controller请求起作用，而过滤器则可以对几乎所有的请求起作用。
        （4）在Controller的生命周期中，拦截器可以多次被调用，而过滤器只能在容器初始化时被调用一次。
    过滤器应用场景:设置编码字符、过滤铭感字符
    拦截器应用场景:拦截未登陆用户、审计日志
    ```
    
### 2.0版本新特性

- `以Java 8 为基准`
    ```
    Spring Boot 2.0 要求 Java 版本必须 8 以上， Java 6 和 7 不再支持。
    ```
- `内嵌容器包结构调整`
    ```
    为了支持reactive使用场景，内嵌的容器包结构被重构了的幅度有点大。EmbeddedServletContainer被重命名为WebServer，
    并且org.springframework.boot.context.embedded 包被重定向到了org.springframework.boot.web.embedded包下。
    举个例子，如果你要使用TomcatEmbeddedServletContainerFactory回调接口来自定义内嵌Tomcat容器，你现在应该使用TomcatServletWebServerFactory。
    ```
- `Servlet-specific 的server properties调整`
    ```
    大量的Servlet专属的server.* properties被移到了server.servlet下：
        Old property	New property
        server.context-parameters.*	server.servlet.context-parameters.*
        server.context-path	server.servlet.context-path
        server.jsp.class-name	server.servlet.jsp.class-name
        server.jsp.init-parameters.*	server.servlet.jsp.init-parameters.*
        server.jsp.registered	server.servlet.jsp.registered
        server.servlet-path	server.servlet.path
    由此可以看出一些端倪，那就是server不再是只有servlet了，还有其他的要加入。
    ```
- `Actuator 默认映射`
    ```
    actuator的端点（endpoint）现在默认映射到/application，比如，/info 端点现在就是在/application/info。但你可以使用management.context-path来覆盖此默认值。
    ```
- `Spring Loaded不再支持`
    ```
    由于Spring Loaded项目已被移到了attic了，所以不再支持Spring Loaded了。现在建议你去使用Devtools。Spring Loaded不再支持了。
    ```
- `支持Quartz Scheduler`
    ```
    Spring Boot 2.0针对Quartz调度器提供了支持。你可以加入spring-boot-starter-quartz starter来启用。而且支持基于内存和基于jdbc两种存储。
    ```
- `OAuth 2.0 支持`
    ```
    Spring Security OAuth 项目中的功能将会迁移到Spring Security中。将会OAuth 2.0。
    ```
- `支持Spring WebFlux`
    ```
    WebFlux 模块的名称是 spring-webflux，名称中的 Flux 来源于 Reactor 中的类 Flux。该模块中包含了对反应式 HTTP、服务器推送事件和
     WebSocket 的客户端和服务器端的支持。对于开发人员来说，比较重要的是服务器端的开发，这也是本文的重点。在服务器端，WebFlux 支持两种
     不同的编程模型：第一种是 Spring MVC 中使用的基于 Java 注解的方式；第二种是基于 Java 8 的 lambda 表达式的函数式编程模型。这两种
     编程模型只是在代码编写方式上存在不同。它们运行在同样的反应式底层架构之上，因此在运行时是相同的。WebFlux 需要底层提供运行时的支持，
     WebFlux 可以运行在支持 Servlet 3.1 非阻塞 IO API 的 Servlet 容器上，或是其他异步运行时环境，如 Netty 和 Undertow。
    ```
- `版本要求`
    ```
    Jetty
        要求Jetty最低版本为9.4。
    Tomcat
        要求Tomcat最低版本为8.5。
    Hibernate
        要求Hibernate最低版本为5.2。
    Gradle
        要求Gradle最低版本为3.4。
    SendGrid
        SendGrid最低支持版本是3.2。为了支持这次升级，username和password已经被干掉了。因为API key现在是唯一支持的认证方式。
    ```
    
### 手写springboot框架

- springboot核心原理
    ```
    基于springmvc无配置文件完全注解化(纯java)+内置tomcat-embed-core来实现springboot框架,main函数启动
    快速整合第三方框架
        原理:Maven继承依赖关系
    完全无配置文件采用注解化
        原理:使用java代码(spring3内置注解)来对springmvc容器进行初始化
    内置web服务器
        原理:Java提供内置Tomcat容器框架,使用java语言创建并操作tomcat容器,加载class文件
    ```
- 内置web服务器
    ```
    1.添加pom依赖
        <!--Java语言操作tomcat -->
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-core</artifactId>
            <version>8.5.16</version>
        </dependency>
        <!-- spring-web -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>5.0.4.RELEASE</version>
            <scope>compile</scope>
        </dependency>
        <!-- spring-mvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.0.4.RELEASE</version>
            <scope>compile</scope>
        </dependency>
        <!-- tomcat对jsp支持 -->
        <dependency>
            <groupId>org.apache.tomcat</groupId>
            <artifactId>tomcat-jasper</artifactId>
            <version>8.5.16</version>
        </dependency>
    2.创建servlet类
        public class IndexServet extends HttpServlet {
        	@Override
        	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        		doPost(req, resp);
        	}
        	@Override
        	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        		resp.getWriter().print("springboot2.0");
        	}
        }
    3.创建tomcat运行
        private static int PORT = 8080;// 端口号
        private static String CONTEXTPATH = "/demo";// 项目名称
    
        public static void main(String[] args) throws LifecycleException {
            // 创建Tomcat服务器
            Tomcat tomcatServer = new Tomcat();
            // 设置Tomcat端口号
            tomcatServer.setPort(PORT);
            tomcatServer.getHost().setAutoDeploy(false);
            // 创建Context上下文
            StandardContext standardContext = new StandardContext();
            standardContext.setPath(CONTEXTPATH);
            standardContext.addLifecycleListener(new FixContextListener());
            // tomcat容器添加standardContext
            tomcatServer.getHost().addChild(standardContext);
            // 创建servlet
            tomcatServer.addServlet(CONTEXTPATH, "IndexServet", new IndexServet());
            // 添加servleturl映射
            standardContext.addServletMappingDecoded("/index", "IndexServet");
            tomcatServer.start();
            tomcatServer.getServer().await();
        }
    ```
- 完全无配置文件采用注解化
    ```
    1.加载SpringMVC的DispatcherServlet
        AbstractAnnotationConfigDispatcherServletInitializer这个类负责配置DispatcherServlet、初始化Spring MVC容器和Spring容器。
        getRootConfigClasses()方法用于获取Spring应用容器的配置文件，这里我们给定预先定义的RootConfig.class；
        getServletConfigClasses负责获取Spring MVC应用容器，这里传入预先定义好的WebConfig.class；
        getServletMappings()方法负责指定需要由DispatcherServlet映射的路径，这里给定的是"/"，意思是由DispatcherServlet处理所有向该应用发起的请求。
        
        public class SpittrWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
        	// 加载根容器
        	protected Class<?>[] getRootConfigClasses() {
        		return new Class[] { RootConfig.class };
        	}
            // 加载SpringMVC容器
        	protected Class<?>[] getServletConfigClasses() {
        		return new Class[] { WebConfig.class };
        	}
        	// SpringMVCDispatcherServlet 拦截的请求
        	protected String[] getServletMappings() {
        		return new String[] { "/" };
        	}
        }
    2.加载SpringMVC容器
        @Configuration
        @EnableWebMvc
        @ComponentScan("top.lvzhiqiang.controller")
        public class WebConfig extends WebMvcConfigurerAdapter {
            // 创建SpringMVC视图解析器
        	@Bean
        	public ViewResolver viewResolver() {
        		InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
        		viewResolver.setPrefix("/WEB-INF/views/");
        		viewResolver.setSuffix(".jsp");
        		viewResolver.setExposeContextBeansAsAttributes(true);//可以在JSP页面中通过${}访问beans
        		return viewResolver;
        	}
        }
    3.加载RootConfig容器
        @Configuration
        @ComponentScan(basePackages = "top.lvzhiqiang")
        public class RootConfig {
        }
    4.运行
        public static void main(String[] args) throws ServletException, LifecycleException {
            start();
        }
        public static void start() throws ServletException, LifecycleException {
            // 创建Tomcat容器
            Tomcat tomcatServer = new Tomcat();
            // 端口号设置
            tomcatServer.setPort(9090);
            // 读取项目路径,主要用来加载一些静态资源
            StandardContext ctx = (StandardContext) tomcatServer.addWebapp("/", new File("src/main").getAbsolutePath());
            // 禁止重新载入
            ctx.setReloadable(false);
            // class文件读取地址
            File additionWebInfClasses = new File("target/classes");
            // 创建WebRoot
            WebResourceRoot resources = new StandardRoot(ctx);
            // tomcat内部读取Class执行
            resources.addPreResources(new DirResourceSet(resources, "/WEB-INF/classes", additionWebInfClasses.getAbsolutePath(), "/"));
            tomcatServer.start();
            // 异步等待请求执行
            tomcatServer.getServer().await();
        }
    ```

## 参考链接

## 结束语

- 未完待续...