# 要学习 springboot 先要学习 spring

## Spring 前置

[spring 原理](https://mp.weixin.qq.com/s?__biz=Mzg2NTAzMTExNg==&mid=2247483724&idx=1&sn=77ce80187dbfdbaaafa0366f6a0c9151&scene=19#wechat_redirect)

为了降低 Java 开发的复杂性，Spring 采用了以下 4 种关键策略：

1、基于 POJO 的轻量级和最小侵入性编程，所有东西都是 bean；

2、通过 IOC，依赖注入（DI）和面向接口实现松耦合；

3、基于切面（AOP）和惯例进行声明式编程；

4、通过切面和模版减少样式代码，RedisTemplate，xxxTemplate；

### 对上面四点的详细解释

1 基于 POJO 的轻量级和最小侵入性编程：
POJO（Plain Old Java Object）是指简单的 Java 对象，它们不需要实现特定的接口或继承特定的类。
Spring 框架允许开发者使用普通的 Java 类来构建应用程序，而不需要依赖于特定的类或接口。这种设计减少了对框架的依赖，使得代码更加轻量级和易于测试。
最小侵入性意味着 Spring 尽可能少地改变现有代码，使得现有的 Java 类可以很容易地集成到 Spring 应用程序中。

2 IoC 的解释看
[Spring-全面详解（学习总结）-CSDN 博客](https://blog.csdn.net/weixin_44207403/article/details/106736102?app_version=5.12.0&code=app_1562916241&csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22106736102%22%2C%22source%22%3A%22halios11%22%7D&uLinkId=usr1mkqgl919blen&utm_source=app)的第一个例子

3 什么是 AOP？
AOP（Aspect Oriented Programming）即面向切面编程，AOP 是 OOP（面向对象编程）的一种延续，二者互补，并不对立。

4 通过切面和模版减少样式代码：
Spring 提供了一系列的模板类（如 JdbcTemplate、HibernateTemplate 等），这些模板封装了重复的代码，提供了一致的编程模型。
通过使用这些模板，开发者可以避免编写大量的样板代码（Boilerplate Code），专注于业务逻辑的实现。
例如，JdbcTemplate 简化了 JDBC 编程，开发者不需要处理连接的打开、关闭和异常处理，只需关注 SQL 语句和结果映射。

#### 对样板代码的补充 (最近的例子就是写多线程的时候会写很多重复又不得不写的 try catch 等等)

样板代码（Boilerplate Code）是指在编程中经常重复出现的标准代码，这些代码通常是固定的模式，没有太多变化，但是每次使用时都需要手动编写。样板代码往往与特定技术或框架无关，它们是在实现某些常见功能时必须编写的代码，例如*错误处理、资源管理、数据验证*等。

样板代码的特点是：

1. **重复性**：它们在不同的程序或模块中频繁出现，形式和结构相似。
2. **通用性**：它们通常不包含业务逻辑，而是提供基础的功能，如数据库连接、异常捕获等。
3. **必要性**：尽管重复，这些代码对于程序的正确运行是必要的。
4. **缺乏灵活性**：样板代码往往不易于修改和扩展，因为它们是为了满足通用需求而设计的。

例如，在 Java 中使用 JDBC 连接数据库时，你需要编写以下**样板代码**：

```java
Connection conn = null;
try {
    // 加载数据库驱动
    Class.forName("com.mysql.jdbc.Driver");
    // 创建数据库连接
    conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "user", "password");
    // 执行数据库操作...
} catch (Exception e) {
    // 异常处理...
} finally {
    // 关闭数据库连接
    if (conn != null) {
        try {
            conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

在上述代码中，连接数据库、处理异常和关闭连接的部分就是样板代码。这些代码在每个使用 JDBC 的程序中都会出现，而且格式几乎相同。
为了减少样板代码，许多框架和库提供了抽象层，例如 Spring 的 `JdbcTemplate`，它封装了创建连接、执行 SQL 语句和处理结果的过程，让你只需要关注 SQL 本身和结果映射。这样，你就可以避免编写重复的 JDBC 代码，专注于业务逻辑的实现。

==========================================================

[Spring 常见面试题总结 | JavaGuide](https://javaguide.cn/system-design/framework/spring/spring-knowledge-and-questions-summary.html#spring-%E5%8C%85%E5%90%AB%E7%9A%84%E6%A8%A1%E5%9D%97%E6%9C%89%E5%93%AA%E4%BA%9B)
[Spring-全面详解（学习总结）-CSDN 博客](https://blog.csdn.net/weixin_44207403/article/details/106736102?app_version=5.12.0&code=app_1562916241&csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22106736102%22%2C%22source%22%3A%22halios11%22%7D&uLinkId=usr1mkqgl919blen&utm_source=app)

## Spring 框架是一个开源的 Java 应用程序框架，它提供了广泛的工具和功能来支持企业级应用程序的开发。Spring 的基础部分通常包括以下几个核心模块：

1. **Spring Core**：

   - 这是 Spring 框架的核心，提供了控制反转（Inversion of Control, IoC）容器，它允许对象之间的依赖关系自动注入，而不是传统的手动创建和管理对象。
   - Spring Core 还提供了其他基本功能，如事件发布、国际化、资源加载和类型转换。

2. **Spring Context**：

   - 建立在 Spring Core 之上，提供了额外的框架功能，如 Spring 应用程序上下文、声明式事务管理和对 Java 持久化 API (JPA) 的支持。
   - Spring Context 模块还包括对 EJB、JMS、邮件服务和调度任务的支持。

3. **Spring AOP (Aspect-Oriented Programming)**：

   - 提供了面向切面编程的支持，允许开发者将横切关注点（如日志、安全、事务等）与业务逻辑分离。
   - 通过使用 AOP，可以模块化地增强应用程序的功能，而不需要修改业务代码。

4. **Spring MVC**：

   - 一个基于模型-视图-控制器设计模式的 Web 框架，用于构建 Web 应用程序和 RESTful Web 服务。
   - 提供了一套工具来处理 HTTP 请求、绑定请求参数、渲染视图和处理表单提交等。

5. **Spring Data**：

   - 提供了数据访问和操作的一致抽象，简化了数据库和数据存储的交互。
   - 支持多种数据存储技术，如 JDBC、JPA、Hibernate、Cassandra 和 MongoDB 等。

6. **Spring Security**：

   - 提供了全面的安全服务，包括认证、授权和保护 Web 应用程序、RESTful API 和方法调用。
   - 支持多种认证机制，如基于表单的认证、OAuth2、OpenID Connect 等。

7. **Spring Boot**：
   - 一个快速应用开发框架，它提供了默认配置和启动机制，使得创建独立的、生产级别的 Spring 应用程序变得非常容易。
   - Spring Boot 旨在简化“配置地狱”，并允许开发者快速启动和运行 Spring 应用程序。

这些基础模块构成了 Spring 框架的核心，为开发者提供了构建复杂企业级应用程序所需的各种工具和服务。通过使用 Spring，开发者可以专注于编写业务逻辑，而不必关心底层的技术细节，从而提高开发效率和代码质量。

## Spring AOP（Aspect-Oriented Programming，面向切面编程）是 Spring 框架中的一个模块，它允许开发者将横切关注点（cross-cutting concerns）从业务逻辑中分离出来。横切关注点是指那些影响多个类或模块的问题，例如日志记录、事务管理、安全性、缓存等。在传统的程序设计中，这些关注点通常散布在各个方法和类中，导致代码重复和难以维护。

AOP 的核心概念是通过“切面”（Aspect）和“通知”（Advice）来模块化这些横切关注点：

1. **切面（Aspect）**：
   切面是一个模块化的横切关注点的实现。它包含了通知和其他 AOP 元素，如切入点（Pointcut）和引入（Introduction）。

2. **通知（Advice）**：
   通知是切面必须执行的动作。在 Spring AOP 中，有五种类型的通知：

   - **前置通知（Before Advice）**：在方法执行之前执行。
   - **后置通知（After Advice）**：在方法执行之后执行，无论方法是否成功。
   - **返回通知（After Returning Advice）**：在方法成功执行之后执行。
   - **异常通知（After Throwing Advice）**：在方法抛出异常时执行。
   - **环绕通知（Around Advice）**：在方法执行前后都执行，可以创建自己的方法执行逻辑。

3. **切入点（Pointcut）**：
   切入点是一个表达式，用于匹配那些需要被通知执行的连接点（Join Points）。连接点是程序执行的某个特定位置，如方法调用或异常抛出的地方。

4. **引入（Introduction）**：
   引入允许开发者为现有的类添加新的方法或属性。

5. **目标对象（Target Object）**：
   目标对象是指那些被通知所通知的对象。

6. **代理（Proxy）**：
   代理是 AOP 容器创建的，用来代替目标对象的对象。当代理对象的方法被调用时，相应的通知会被执行。

Spring AOP 支持两种类型的代理：

- **基于代理的 AOP**：使用 JDK 动态代理或 CGLIB 库创建代理对象。这种方式可以为目标对象创建子类或实现接口的代理。
- **基于 AspectJ 的 AOP**：AspectJ 是一个更全面的 AOP 框架，它提供了更强大的切入点表达式和更多的 AOP 特性。Spring 可以与 AspectJ 集成，利用 AspectJ 的编译时或加载时织入功能。

Spring AOP 的使用通常涉及以下步骤：

1. 定义切面和通知。
2. 配置切入点来指定通知应该织入的连接点。
3. 在 Spring 配置文件中注册切面和相关的 Bean。
4. 运行时，Spring AOP 容器会根据配置创建代理，并在适当的时机执行通知。

通过使用 Spring AOP，开发者可以更清晰地分离和重用横切关注点的代码，从而提高代码的模块化和可维护性。

## Spring MVC（Model-View-Controller）是 Spring 框架中的一个模块，它实现了 Model 2 架构模式，用于构建基于 Java 的 web 应用程序。Spring MVC 提供了一个干净的、模块化的方式来处理 HTTP 请求和响应，使得开发者可以专注于业务逻辑，同时保持视图技术的灵活性。

以下是 Spring MVC 的核心组件和概念：

1. **DispatcherServlet**：

   - 这是 Spring MVC 的前端控制器（Front Controller），负责处理所有进入应用程序的 HTTP 请求。DispatcherServlet 根据请求的 URL 和配置的映射规则来决定哪个控制器（Controller）应该处理请求。

2. **Controller**：

   - 控制器是处理 HTTP 请求的组件。它们通常使用 `@Controller` 注解进行标记，并包含处理特定请求的方法。这些方法被称为动作（Action）或处理器方法（Handler Method），它们负责处理用户的输入，调用业务逻辑，并返回视图名称或数据模型。

3. **Model**：

   - Model 代表应用程序的数据模型，它可以是 JavaBeans、POJOs 或任何其他数据对象。控制器处理请求后，会将数据模型添加到 ModelAndView 对象中，以便视图可以访问。

4. **View**：

   - View 是用于呈现数据的组件，通常是 JSP、Thymeleaf、Freemarker 或其他模板引擎。视图根据控制器提供的模型数据来渲染最终的页面。

5. **RequestMapping**：

   - 这是一个用于映射 HTTP 请求到控制器方法的注解。它可以应用于控制器类或方法上，以定义 URL 模式和请求方法（GET、POST 等）。

6. **RequestBody** 和 **ResponseBody**：

   - `@RequestBody` 用于将 HTTP 请求体中的原始数据绑定到动作方法的参数上。
   - `@ResponseBody` 用于将动作方法的返回值直接写入 HTTP 响应体中，而不是渲染一个视图。

7. **ModelAndView**：

   - ModelAndView 是一个模型-视图对象，它包含了视图名称和附加属性。控制器方法可以返回一个 ModelAndView 对象，它告诉 DispatcherServlet 应该使用哪个视图来渲染响应，并且可以包含传递给视图的数据。

8. **ViewResolver**：

   - 视图解析器负责将逻辑视图名称解析为实际的视图模板。它通常根据视图名称的后缀来选择合适的模板引擎。

9. **数据绑定和验证**：

   - Spring MVC 支持 HTTP 请求参数到 Java 对象的自动数据绑定。它还集成了数据验证框架，如 Hibernate Validator，以便于执行验证逻辑。

10. **异常处理**：
    - Spring MVC 提供了异常处理机制，允许开发者定义全局或局部的异常处理器来处理和转换异常。

Spring MVC 的工作流程通常如下：

1. 用户发起 HTTP 请求，DispatcherServlet 接收请求。
2. DispatcherServlet 根据请求的 URL 找到对应的控制器和动作方法。
3. 动作方法处理请求，可能调用业务逻辑层，并返回 ModelAndView 对象。
4. DispatcherServlet 使用 ViewResolver 找到对应的视图模板。
5. 视图模板使用模型数据渲染最终的页面。
6. DispatcherServlet 将渲染好的页面作为 HTTP 响应返回给用户。

Spring MVC 的设计使得 web 应用程序的开发变得更加模块化和灵活，同时保持了高性能和低侵入性。开发者可以轻松地集成其他 Spring 框架的功能，如安全性、数据访问和消息传递等。

### 在 Spring 框架中，Controller 是一个核心组件，它属于 Spring MVC 模块，用于处理来自客户端的 HTTP 请求，并返回适当的响应。Controller 充当应用程序的前端，负责接收用户的输入，调用相应的服务层逻辑，并返回视图或数据给客户端。

以下是 Spring Controller 的一些关键特性和概念：

1. **@Controller 注解**：

   - 这个注解用于标记一个类作为 Spring MVC 控制器。被标记的类通常包含处理 HTTP 请求的方法。

2. **@RequestMapping 注解**：

   - 用于映射 HTTP 请求到控制器中的具体方法上。可以指定 URL 模式、HTTP 方法（GET、POST 等）、请求参数等。
   - 可以应用于控制器类或方法上。如果应用于类上，它定义了类中所有方法的基 URL。

3. **处理方法**：

   - 控制器中的方法通常被称为动作方法或处理器方法。这些方法使用 `@RequestMapping` 或其派生注解（如 `@GetMapping`, `@PostMapping` 等）进行映射。
   - 动作方法可以接收请求参数、请求头、表单数据等，并返回一个视图名称或直接返回数据。

4. **返回视图**：

   - 控制器方法通常返回一个视图名称，这个名称由视图解析器转换为实际的视图页面。
   - 视图可以是 JSP、Thymeleaf、Freemarker 等模板引擎渲染的页面。

5. **返回数据**：

   - 控制器方法也可以返回数据，例如 JSON 或 XML。这通常通过 `@ResponseBody` 注解实现，表示方法的返回值应该直接写入 HTTP 响应体中。

6. **路径变量**：

   - `@RequestMapping` 支持路径变量，允许控制器方法接收 URL 中的特定部分作为参数。
   - 路径变量通过 `{variableName}` 定义，并在控制器方法中作为参数自动注入。

7. **表单处理**：

   - 控制器可以处理表单提交，通过 `@RequestParam` 注解或 JavaBean 参数绑定来获取表单数据。
   - 支持文件上传和复杂的表单数据绑定。

8. **异常处理**：

   - 控制器可以定义异常处理方法，通过 `@ExceptionHandler` 注解标记，用于处理特定类型的异常。
   - 可以返回错误视图或错误数据给客户端。

9. **跨域请求**：

   - 通过 `@CrossOrigin` 注解，控制器可以配置跨域资源共享（CORS）支持，允许来自不同源的 AJAX 请求。

10. **安全性**：
    - 通过与 Spring Security 集成，控制器可以实施安全策略，如认证、授权和 CSRF 保护。

Spring Controller 的一个典型例子：

```java
@Controller
public class MyController {

    @GetMapping("/greeting")
    public String greeting(Model model) {
        model.addAttribute("message", "Hello, World!");
        return "greeting";
    }

    @PostMapping("/submitForm")
    public String submitForm(@ModelAttribute("formData") MyFormData formData) {
        // 处理表单数据
        return "result";
    }
}
```

在这个例子中，`MyController` 类用 `@Controller` 注解标记，它有两个方法：`greeting` 处理 GET 请求并添加一个属性到模型中，`submitForm` 处理 POST 请求并接收一个表单数据对象。

Spring Controller 使得开发基于 HTTP 的应用程序变得简单和直观，同时提供了强大的功能来处理各种 Web 应用程序的需求。通过使用 Spring MVC，开发者可以构建灵活、可扩展和易于维护的 Web 应用程序。

### `@RestController` 是 Spring MVC 中的一个特殊注解，它是一个组合注解，用于创建 RESTful Web 服务的控制器。这个注解是 `@Controller` 和 `@ResponseBody` 两个注解的结合体。

当你在一个类上使用 `@RestController` 注解时，它表明该类的所有方法都默认使用 `@ResponseBody` 行为，即方法的返回值会自动转换为 JSON 或 XML 等格式的响应体，直接写入 HTTP 响应中。这样，你就不需要在每个处理请求的方法上单独添加 `@ResponseBody` 注解。

`@RestController` 的主要特点和用途如下：

1. **自动序列化**：控制器方法的返回值会被自动序列化为 JSON 或 XML 格式，这通常通过 Spring 的消息转换器（如 Jackson 或 JAXB）来实现。

2. **无需视图解析**：与 `@Controller` 不同，`@RestController` 不涉及视图技术，它不返回视图名称，而是直接返回数据。

3. **简化代码**：由于不需要编写视图相关的代码，你可以更专注于业务逻辑和数据模型，使得控制器代码更加简洁。

4. **RESTful 服务**：`@RestController` 通常用于创建遵循 REST 架构风格的 Web 服务，它支持无状态、客户端-服务器架构、缓存、分层系统等 REST 原则。

下面是一个使用 `@RestController` 的简单示例：

```java
package com.example.demo1.control;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/*@RestController
表明这个类是一个 Spring MVC 控制器，并且所有的方法返回值都会自动转换为 HTTP 响应体。
@RestController 是 @Controller 和 @ResponseBody 的一个便捷组合，通常用于创建 RESTful Web 服务。
* */
@RestController
public class helloController {
    /*
    * @RequestMapping("/hello")：
        这是一个方法级别的注解，用于将 HTTP 请求映射到控制器方法上。
        在这个例子中，它将 HTTP 请求的 /hello 路径映射到 hello 方法上。
    * */
    @RequestMapping("/hello")
    public String hello(){
        //方法的返回类型是 String，这意味着方法的返回值将作为文本响应返回给客户端。
        return "你好 测试一下Hello，需要输入http://localhost:8080/hello,hello来自上面的请求";
    }
    @RequestMapping("hi")
    public String hi(){ return "hi"; }
}

```

在这个例子中，`GreetingController` 类用 `@RestController` 注解标记，`getGreeting` 方法返回一个 `Greeting` 对象。当客户端请求 `/greeting` 路径时，`getGreeting` 方法会被调用，并且返回的对象会被自动转换为 JSON 格式的响应体。

`@RestController` 是创建现代 Web 应用程序和服务时非常有用的工具，特别是当你需要快速开发 RESTful API 时。它简化了开发过程，并帮助开发者遵循 REST 架构的最佳实践。

[效果如图](./pictures/图1.png)

## Spring DAO（Data Access Object）是 Spring 框架中用于简化数据访问层实现的一个模式。DAO 模式是一种常见的设计模式，它提供了一个抽象层，用于封装数据库访问的逻辑，使得应用程序可以通过统一的接口与数据库交互，而不需要关心具体的实现细节。

在 Spring 中，DAO 模式通常与 Spring 的几个核心功能结合使用，以提高数据访问层的可维护性和可测试性：

1. **Spring JDBC**：

   - Spring 提供了一个 JDBC 抽象层，它简化了 JDBC 编程的复杂性，如资源管理、错误处理和 SQL 语句的执行。
   - Spring JDBC 支持使用简单的 JDBC 模板类（如 `JdbcTemplate` 和 `NamedParameterJdbcTemplate`）来执行 SQL 语句和处理结果。

2. **Spring ORM**：

   - Spring 提供了对流行 ORM 框架（如 Hibernate、JPA 和 MyBatis）的集成支持。
   - 通过 Spring 的 ORM 集成，开发者可以更容易地配置和使用 ORM 框架，同时保持代码的干净和可维护性。

3. **事务管理**：

   - Spring 提供了强大的事务管理功能，它支持声明式事务和编程式事务。
   - 通过使用 `@Transactional` 注解或 XML 配置，开发者可以轻松地管理事务的边界和传播行为，确保数据的一致性和完整性。

4. **DAO 接口和实现**：

   - DAO 接口定义了数据访问的方法，而实现类提供了这些方法的具体实现。
   - 在 Spring 中，DAO 实现类通常使用 `@Repository` 注解进行标记，以便 Spring 容器可以识别它们并将它们作为 bean 进行管理。

5. **数据访问异常**：

   - Spring 提供了一套异常层次结构，用于处理数据访问层中发生的异常。
   - 这些异常类（如 `DataAccessException`）提供了更具体的异常信息，使得错误处理更加直观和方便。

6. **Spring Data**：
   - Spring Data 是 Spring 框架的一个子项目，它提供了一系列的框架，用于简化数据访问层的开发。
   - 例如，Spring Data JPA 提供了一种基于 Repository 接口的编程模型，开发者只需定义接口，Spring Data JPA 就会自动实现这些接口的方法。

在 Spring 中实现 DAO 模式通常涉及以下步骤：

1. 定义 DAO 接口，声明数据访问方法。
2. 实现 DAO 接口，提供具体的 SQL 操作和数据映射。
3. 使用 `@Repository` 注解标记 DAO 实现类，使其成为 Spring 容器的 bean。
4. 在服务层或业务层中注入 DAO 接口，并调用其方法进行数据操作。
5. 配置事务管理器和事务属性，确保事务的正确执行。

通过使用 Spring DAO，开发者可以更容易地实现数据访问层，同时保持代码的清晰、灵活和可维护。Spring 的数据访问和事务管理功能使得与数据库的交互更加高效和安全。

## [补充：Java 单例模式，看这一篇就够了](https://zhuanlan.zhihu.com/p/580095502)

特点 ↓

1.  唯一性：单例类只能创建一个实例，无论多少次尝试创建，都会得到同一个对象。
2.  全局访问点：可以通过一个统一的接口或变量来访问这个唯一的实例。
3.  线程安全：在多线程环境中，需要确保对单例实例的访问是线程安全的，以防止多个线程同时创建多个实例。
4.  延迟初始化：单例实例可以在第一次使用时才创建，这样可以延迟加载资源和初始化过程。
