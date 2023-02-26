# SpringMVC 初识（入门案例）
## MVC模式介绍
关于什么是MVC模式我在学习Java Web的时候写过一篇文章：MVC模式设计思想 。MVC模式的全名是Model-View-Controller，即模型(Model )－视图(View )－控制器(Controller)的缩写。首先要明确的一点是：MVC模式它不是类，也不是什么框架，它只是一种开发的设计思想。将业务逻辑、数据处理、界面显示分别抽取出来统一放到一个地方，从而使同一个程序可以使用不同的表现形式。

所以MVC的的程序分为三个核心的模块，这三个模块的详细介绍如下：

- **模型（Model）**：负责封装与应用程序的业务逻辑相关的数据以及对数据的处理方法。模型层有对数据直接访问的权力，例如对数据库的访问。它不关心它会如何被视图层显示或被控制器调用，它只接受数据并处理，然后返回一个结果。
- **视图（View）**：负责应用程序对用户的显示，它从用户那里获取输入数据并通过控制层传给业务层处理，然后再通过控制层获取业务层返回的结果并显示给用户。
- **控制器（Controller）**：负责控制应用程序的流程，它接收从视图层传过来的数据，然后选择Model层中的某个业务来处理，接收Model层返回的结果并选择视图层中的某个视图来显示结果。

## 第一步：导入SpingMVC坐标与Servlet坐标
```xml
<dependencies>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.10.RELEASE</version>
    </dependency>
</dependencies>
```

## 第二步：初始化SpingMVC环境（同Spring环境）
```java
@Configuration
public class SpringMvcConfig {
}
```

从Spring3.0，`@Configuration`用于定义配置类，可替换xml配置文件，被注解的类内部包含有一个或多个被@Bean注解的方法，这些方法将会被`AnnotationConfigApplicationContext`或`AnnotationConfigWebApplicationContext`类进行扫描，并用于构建bean定义，初始化Spring容器。

## 第三步：创建SpringMVC控制类容器（等同Servlet功能）
```java
@Controller
public class UserController {
    @RequestMapping("/save")
    @ResponseBody
    public String save(){
        System.out.println("Save request");
        return "{'info':'zihao'}";
    }
}
```

`@Controller`: 用来定义接口。

`@RequestMapping`: 做映射的路径。 被标记的方法会被分发处理器扫描识别，将不同的请求分发到对应的接口上。

`@ResponseBody`：将不再返回HTML标签的页面，而是转化为其他数据格式，例如xml，json等

## 第四步：设置SpringMVC加载对应的bean
```java
@Configuration
@ComponentScan("com.zihao.controller")
public class SpringMvcConfig {
}
```

`@ComponentScan`：通常和@Configuration注解一起使用，主要的作用就是定义包扫描的规则，然后根据定义的规则找出哪些需类需要自动装配到spring的bean容器中，然后交由spring进行统一管理。说明：针对标注了@Controller、@Service、@Repository、@Component 的类都可以别spring扫描到。

## 第五步：初始化Servlet容器，加载SpringMVC环境，并设置SpringMVC请求拦截的路径
```java
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
    // 在这里加载springMVC对应的容器对象
    protected WebApplicationContext createServletApplicationContext() {
        // 加载配置类
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }

    // 规定哪一些归SpringMVC处理，或Spring处理
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    // 加载Spring对应的容器对象，所以在这里我们不需要
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }
}
```

`AbstractDispatcherServletInitializer`类是由SpringMVC提供的快速初始化Web3.0容器的抽象类。

他提供三个接口方法：

`createServletApplicationContext()`： 创建Servlet容器时，加载SpringMVC对应的bean并放入WebApplicationContext对象范围中，而WebApplicationContext的作用范围就是ServletContext范围，即真个web容器的范围。

`getServletMappings()`：设定SpingMVC对应的请求映射路径，设置为'/'表示拦截所有请求，任意请求都将转入到SpingMVC进行处理。

`createRootApplicationContext()`：如果需要加载非SpringMVC对应的bean，使用当前方法。使用的方式同createServletApplicationContext()

## 注意事项
```xml
<build>
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.1</version>
        <configuration>[upload.sh](..%2Fupload.sh)
          <port>8080</port>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
</build>
```

完成以上步骤后，根据`pom.xml`设置的路径就可以尝试发起请求。
例如：`localhost:8080/save`