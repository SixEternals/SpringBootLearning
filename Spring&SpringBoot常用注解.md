[参考自](https://javaguide.cn/system-design/framework/spring/spring-common-annotations.html)

# 1. @SpringBootApplication

这里先单独拎出`@SpringBootApplication` 注解说一下，虽然我们一般不会主动去使用它。Guide：这个注解是 Spring Boot 项目的基石，创建 SpringBoot 项目之后会默认在主类加上。

```java
@SpringBootApplication
public class SpringSecurityJwtGuideApplication {
      public static void main(java.lang.String[] args) {
        SpringApplication.run(SpringSecurityJwtGuideApplication.class, args);
    }
}
```

我们可以把 `@SpringBootApplication`看作是 `@Configuration`、`@EnableAutoConfiguration`、`@ComponentScan` 注解的集合。

根据 SpringBoot 官网，这三个注解的作用分别是：

1. `@EnableAutoConfiguration`：启用 SpringBoot 的自动配置机制
2. `@ComponentScan`：扫描被`@Component` (`@Repository`,`@Service`,`@Controller`)注解的 `bean`，注解默认会扫描该类所在的包下所有的类。
3. `@Configuration`：允许在 Spring 上下文中注册额外的 bean 或导入其他配置类

# Spring Bean 相关

## ① `@Autowired`

自动导入对象到类中，被注入进的类同样要被 Spring 容器管理比如：Service 类注入到 Controller 类中。

```java
@Service
public class UserService {
  ......
}

@RestController
@RequestMapping("/users")
public class UserController {
   @Autowired
   private UserService userService;
   ......
}
```

省流解释:@Autowired 是直接把 userService 创建出来之后交付给 spring 管理

详细解释：
`@Autowired` 注解是由 Spring 框架提供的，用于实现依赖注入。当你在一个字段、构造函数或方法上使用 `@Autowired` 注解时，Spring 容器会在创建 `UserController` 的实例时自动注入一个 UserService 类型的 `bean`。

在这个例子中，`UserService` 类会被实例化并交给 Spring 容器管理，然后 Spring 容器会将这个实例注入到 `UserController` 类的 `userService` 字段中。这意味着 `UserController` 可以无需手动创建 `UserService` 实例就能**直接使用**它，从而实现了控制反转（IoC）和依赖注入（DI）。

## ② `@Component,@Repository,@Service, @Controller`

`@Component` 通用注解
`@Repository` Dao 层
`@Service` 服务层
`@Controller` Spring MVC 控制层

## ③ `@RestController`

`@RestController`注解是`@Controller`和`@ResponseBody`的合集,表示这是个控制器 bean,并且是将函数的返回值直接填入 HTTP 响应体中,是 REST 风格的控制器。

Guide：现在都是前后端分离，说实话我已经很久没有用过@Controller。如果你的项目太老了的话，就当我没说。

单独使用 `@Controller` 不加 `@ResponseBody`的话一般是用在要返回一个视图的情况，这种情况属于比较传统的 Spring MVC 的应用，对应于前后端不分离的情况。`@Controller `+`@ResponseBody` 返回 JSON 或 XML 形式数据关于`@RestController` 和 `@Controller`的对比，请看这篇文章：[链接](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485544&idx=1&sn=3cc95b88979e28fe3bfe539eb421c6d8&chksm=cea247a3f9d5ceb5e324ff4b8697adc3e828ecf71a3468445e70221cce768d1e722085359907&token=1725092312&lang=zh_CN#rd)

简而言之，`@RestController` 是为创建 RESTful 服务而设计的，它简化了返回数据的处理，而 `@Controller` 更适用于传统的 Web 应用程序，提供了更多的灵活性来处理视图和数据模型。在实际开发中，选择哪一个注解取决于你的应用程序需求和设计目标。
当然，以下是 `@RestController` 和 `@Controller` 的使用示例：

#### 使用 `@RestController` 创建 RESTful 服务：

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserRESTController {

    // 这个 GET 请求会返回用户的列表，响应体直接序列化为 JSON
    @GetMapping("/users")
    public List<User> getAllUsers() {
        // 这里应该包含获取用户列表的逻辑
        return new ArrayList<>();
    }
}
```

在这个例子中，`UserRESTController` 类被标记为 `@RestController`，这意味着所有方法的返回值都会自动转换为 JSON 响应。`getAllUsers` 方法处理 `GET` 请求并返回一个用户列表，这个列表会被自动序列化为 JSON 格式。

#### 使用 `@Controller` 创建传统的 Web 应用程序：

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class UserWebController {

    // 这个 GET 请求会返回一个视图名称，并将用户列表传递给视图
    @GetMapping("/users")
    public ModelAndView getUsers() {
        ModelAndView modelAndView = new ModelAndView("users");
        // 这里应该包含获取用户列表的逻辑
        modelAndView.addObject("users", new ArrayList<>());
        return modelAndView;
    }
}
```

在这个例子中，`UserWebController` 类被标记为 `@Controller`，`getUsers` 方法处理 `GET` 请求并返回一个 `ModelAndView` 对象，这个对象包含了视图名称和模型数据。`users` 视图将接收到一个空的用户列表，并根据这个列表渲染页面。

# @Qualifier

果容器中有多个 `Dog` 类型的 `bean`，`@Qualifier` 注解可以作为后备机制，允许你指定一个默认的 `bean` 名称，这样即使没有通过 `@Autowired` 注解明确指定 `bean` 的名称，Spring 也知道应该注入哪一个 `Dog bean`。

例如，如果你有以下两个 Dog 类型的 bean 定义：

```java
@Service("dog1")
public class Dog1 implements Dog {
    // ...
}

@Service("dog2")
public class Dog2 implements Dog {
    // ...
}
```

你可以使用 @Qualifier 来指定要注入哪个具体的 Dog 实现：

```
@Autowired
@Qualifier("dog1")
private Dog dog;
```

# @Validated 数据校验

`@Validated` 是 Spring Framework 中的一个注解，用于在运行时对对象进行数据校验。它是 JSR 380 规范的一部分，该规范定义了 Bean Validation API，旨在对 Java 对象进行验证。Spring 通过集成这个 API，使得开发者能够轻松地对用户输入或其他数据进行校验。

## ① 何时使用 `@Validated`

- **用户输入校验**：当你需要确保用户提交的数据符合预期的格式或范围时，比如注册表单的邮箱地址、密码强度等。
- **业务逻辑校验**：在执行复杂业务逻辑前后，你可能需要校验数据的完整性和正确性。
- **数据传输校验**：在微服务架构中，服务间传输的数据也需要进行校验，以确保数据的一致性和准确性。

## ② 如何使用 `@Validated`

在使用 `@Validated` 注解之前，你需要添加 Bean Validation API 的依赖到你的项目中。Spring Boot 已经包含了这个依赖，所以通常你不需要额外添加。

接下来，你可以在需要校验的类或方法上使用 `@Validated` 注解。例如：

```java
import org.springframework.validation.annotation.Validated;

@Validated
public class UserDTO {
    @NotNull(message = "Username cannot be null")
    private String username;

    @Size(min = 6, message = "Password must be at least 6 characters long")
    private String password;

    // getters and setters
}
```

在上面的例子中，`UserDTO` 类有两个字段：`username` 和 `password`。`@Validated` 注解确保在创建 `UserDTO` 对象时，这两个字段都会经过校验。

或者

```java
@Component //注册bean
@ConfigurationProperties(prefix = "person") // 导入配置
@Validated  //数据校验
public class Person {
    @Email(message="邮箱格式错误") // 在注入时/使用set方法修改值时若非邮箱地址则报错，message为自定义错误消息
    private String name;
}  // 运行结果 ：default message [不是一个合法的电子邮件地址];
```

## ③ 校验注解

`@Validated` 注解通常与其他校验注解一起使用，这些注解定义了具体的校验规则。以下是一些常用的校验注解：

```java
@NotNull(message="名字不能为空")
private String userName;
@Max(value=120,message="年龄最大不能查过120")
private int age;
@Email(message="邮箱格式错误")
private String email;

空检查
@Null       验证对象是否为null
@NotNull    验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   检查约束元素是否为NULL或者是EMPTY.

Booelan检查
@AssertTrue     验证 Boolean 对象是否为 true
@AssertFalse    验证 Boolean 对象是否为 false

长度检查
@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内
@Length(min=, max=) string is between min and max included.

日期检查
@Past       验证 Date 和 Calendar 对象是否在当前时间之前
@Future     验证 Date 和 Calendar 对象是否在当前时间之后
@Pattern    验证 String 对象是否符合正则表达式的规则
```

.......等等
除此以外，我们还可以自定义一些数据校验规则

## ④ 校验过程

当 Spring 容器创建或处理一个带有 `@Validated` 注解的对象时，它会执行校验逻辑。如果校验失败，将会抛出 `MethodArgumentNotValidException` 或 `ConstraintViolationException` 异常。这些异常可以被 Spring 的异常处理机制捕获，并根据需要进行处理，比如返回错误信息给客户端。

## ⑤ 结论

`@Validated` 注解是 Spring Boot 提供的一种强大且灵活的数据校验机制。通过使用它，你可以确保应用程序接收和处理的数据是有效和可靠的，从而提高应用程序的健壮性和用户体验。
