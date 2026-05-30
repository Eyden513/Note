# Spring、Spring MVC和SpringBoot的关系
**Spring**:是一个==开源的轻量级应用程序框架==，提供了全面的编程和配置模型，==核心是控制反转(IOC)和面向切面编程(AOP)==。它==能管理对象的生命周期和对象间的依赖关系，让开发者专注业务逻辑。==像开发企业级应用时，可借助Spring管理各种组件，整合不同技术框架。
**Spring MVC**:==是Spring框架的一部分==，基于MVC(==Model -View-Controller==，模型 -视图-控制器)架构模式,用于构建Web应用程序。它==分离了业务逻辑、数据展示和用户交互==，使代码更易维护和扩展。比如开发Web项目时，用SpringMVC处理HTTP请求、返回响应数据等。
**SpringBoot**:是==基于Spring构建的快速开发框架==，目的是==简化Spring应用的初始搭建及开发过程==。它默认配置了很多常用功能，如内置Web服务器、自动配置依赖等，遵循“==约定优于配置==”原则，能让开发者快速上手并开发出生产级别的应用。例如新建项目时，使用SpringBoot能快速搭建好基础环境，无需大量手动配置。
# IOC(控制反转)
IOC是一种==设计思想==，指将==对象的创建和管理控制权==从应用程序代码==转移到外部容器==(即IOC容器，如==Spring容器==)。在Spring中，开发者只需在配置文件或使用注解声明对象及依赖关系，容器负责创建对象实例、注入依赖等。比如定义一个Userservice类，通过 @Autowired 注解让Spring容器自动注入其依赖的 UserRepository，而不用在UserService内部手动创建 UserRepository实例，降低了代码耦合度。
#  Spring Bean
==Spring Bean是被Spring IOC容器管理的对象==。这些对象在Spring容器中创建、初始化、销毁，其定义可通过XML配置文件、注解(如@Component、@Service等)或Java配置类(@Configuration搭配@Bean)来声明。==例如使用@Service注解标注的业务层类，就会被Spring容器识别为一个Bean并进行管理。==
# Bean的生命周期
1. 实例化:Spring容器根据配置(如XML、注解)创建Bean的实例，利用==Java反射机制==完成对象创建。
2. 属性赋值:容器通过调用Setter方法或构造函数参数注入等方式，给Bean的属性赋值。
3. 初始化:属性赋值后，若配置了初始化方法(如@Bean注解中指定initMethod，或实现InitializingBean接口)，容器会调用该方法进行初始化操作。
4. 使用:Bean初始化完成后可供应用程序其他组件使用。
5. 销毁:当Bean不再需要(如容器关闭时)，若配置了销毁方法(如@Bean注解中指定destroyMethod，或实现DisposableBean接口)，容器会调用销毁方法清理资源。
# AOP(面向切面编程)
AOP是一种编程范式，==用于处理程序中横切关注点(影响多个模块的功能，如日志记录、事务管理、安全验证等)==。在Spring AOP中:
- 切面(Aspect):==封装横切关注点的模块化单元==(切面类)，由切点(Pointcut)和通知(Advice)组成。
- 切点:定义在程序执行过程中==应用通知的位置(@Pointcut)==，用表达式语言指定，如匹配特定方法名、参数类型等。
- 通知:==在切点处执行的具体代码逻辑==，有前置通知(Before)、后置通知(After Returning)、异常通知(After Throwing)、最终通知(After Finally)、环绕通知(Around )等类型。比如用AOP实现日志记录,在方法执行前后记录日志信息。
```
// LogAspect.java
@Aspect          // 声明这是一个切面
@Component       // 交给Spring管理
public class LogAspect {
    
    // ========== 切点定义 ==========
    // 匹配 UserService 所有方法
    @Pointcut("execution(* com.example.service.UserService.*(..))")
    public void userServicePointcut() {}
    
    // ========== 前置通知 ==========
    @Before("userServicePointcut()")
    public void beforeAdvice(JoinPoint joinPoint) {
        String methodName = joinPoint.getSignature().getName();
        Object[] args = joinPoint.getArgs();
        System.out.println("[前置通知] 方法: " + methodName + ", 参数: " + Arrays.toString(args));
    }
    
    // ========== 后置通知（方法正常返回）==========
    @AfterReturning(pointcut = "userServicePointcut()", returning = "result")
    public void afterReturningAdvice(JoinPoint joinPoint, Object result) {
        String methodName = joinPoint.getSignature().getName();
        System.out.println("[后置通知] 方法: " + methodName + ", 返回值: " + result);
    }

}
```
# 事务
在Spring中，事务是==保证一组操作要么全部成功提交到数据库，要么全部失败回滚的机制==，用于确保数据的一致性和完整性。例如在银行转账中，从一个账户扣款和向另一个账户存款这两个操作需在一个事务中，保证不会出现扣款成功但存款失败的情况。
## 事务隔离界别
1. DEFAULT:Spring默认隔离级别，使用数据库默认的事务隔离级别。
2. READ_UNCOMMITTED:最低隔离级别，==允许一个事务读取另一个事务未提交的数据==，可能导致脏读、不可重复读和幻读问题。
3. READ_COMMITTED:==保证一个事务修改的数据提交后，另一个事务才能读取==，可避免脏读，但仍可能出现不可重复读和幻读。（大多数业务场景下够用）
4. REPEATABLE_READ:可防止脏读和不可重复读，在==同一事务内多次读取相同数据结果一致，但可能存在幻读。==
5. SERIALIZABLE:最高隔离级别，事务按顺序执行，能防止脏读、不可重复读和幻读。
**脏读**：读到了别人还没确认的数据（可能回滚，白高兴一场）。  
**不可重复读**：同一条记录，两次读内容不同（数据被改了）。  
**幻读**：同一个查询，两次读行数不同（数据被新增了/删除了）。
## 实现事务的方式 
1. **编程式事务管理**
在==代码中手动调用事务管理相关方法，如beginTransaction()、commit()、rollback()等==。例如使用TransactionTemplate事务模板对象，先配置PlatformTransactionManager,再用 TransactionTemplate 的 execute 方法处理事务逻辑,在TransactionCallback接口实现中编写具体操作。
2. **声明式事务管理**
- 基于 TransactionProxyFactoryBean:通过配置 TransactionProxyFactoryBean 来代理目标对象,设置事务属性。
- ==基于@Transactional 注解==:在方法或类上添加@Transactional注解，Spring会自动处理事务相关操作，是常用方式。
- 基于AspectjAOP配置事务:配置事务通知(tx:advice)和织入(aop:config)，指定哪些方法应用事务。
# SpringBoot启动流程
1. **加载启动类**:找到并加载标注了@SpringBootApplication的主启动类，该注解组合了@ComponentScan(扫描组件)、@Configuration(配置类)、@EnableAutoconfiguration (启用自动配置)功能。
2. **初始化Spring容器**:创建并初始化Spring应用上下文(Applicationcontext)，准备好容器环境。
3. **自动配置**:@EnableAutoconfiguration发挥作用，Spring Boot根据类路径下的依赖、配置属性等，自动配置应用所需的Bean，如数据库连接池、Web服务器相关配置等。==(Spring Boot自动配置好的)==
4. **加载用户配置**:扫描并加载用户自定义的配置类、Bean定义(如@Configuration类、@Component及其子注解标注的类)。==(用户自己写的代码)==
5. **启动嵌入式服务器(若有Web应用)**:如果是Web项目，启动内置的Web服务器(如Tomcat、Jetty)，监听指定端口等待接收请求。
6. **完成启动**:所有配置和初始化完成，应用启动成功，可对外提供服务。
# Spring Boot常用注解
1. ==@SpringBootApplication==:标注在==主类上==,组合了 @ComponentScan、@Configuration、@EnableAutoconfiguration，开启组件扫描、配置支持和自动配置功能。
2. @Component:通用的Bean定义注解，==被其标注的类会被Spring容器扫描并管理==。@Service (业务层)、@Repository(数据访问层)、@Controller(控制层)是其衍生注解，功能类似，用于更明确分层。
3. @Autowired:==按类型自动注入依赖的Bean==，可用于成员变量、方法、构造函数。也可搭配==@Qualifier按名称注入==。
4. @Resource:按名称装配Bean，是J2EE注解，与@Autowired区别在于匹配方式，==@Resource默认按名称，@Autowired 默认按类型。==
5. @Configuration :标注配置类，等价于传统Spring的XML配置文件，==常与@Bean搭配==，用@Bean注解方法来定义Bean。
6. @Bean:在@Configuration类的方法上使用，==声明该方法返回的对象是一个Bean==，由Spring容器管理。
7. @Scope:定义Bean的作用域，如singleton (单例，默认)、prototype (多例)、request (一次HTTP请求内有效，Web应用)、session(一个用户会话内有效，Web应用)等

| 作用域           | 生命周期        | 实例数   | 典型场景       |
| ------------- | ----------- | ----- | ---------- |
| **singleton** | 容器启动 → 容器销毁 | 1个    | 配置服务、工具类   |
| **prototype** | 每次获取/注入     | N个    | 有状态对象、报表生成 |
| **request**   | 一次 HTTP 请求  | 每请求1个 | 请求追踪、上下文   |
| **session**   | 一次用户会话      | 每会话1个 | 购物车、登录状态   |

8. @Value:从==配置文件读取值注入到变量==。
9. @ConfigurationProperties:将==配置文件属性==批量绑定到==对象==。
10. @ConditionalonProperty:基于配置属性条件装配Bean，满足==指定属性条件才创建Bean==。
```java
@Bean
@ConditionalOnProperty(
    name = "cache.enabled",
    havingValue = "true",
    matchIfMissing = true  // 没配置也创建（默认开启）
)
public CacheManager cacheManager() {
    return new CacheManager();
}
```