<!-- 初版 2023年07月20日 17:10:54
地址 http://3ms.huawei.com/km/blogs/details/14492757
-->


IoC: Inversion of Controll
DI: Dependency Injection

## Spring Bean是实现IoC和DI的基础

Spring Bean是Spring的基础，Bean就是Spring IoC容器在运行时管理的对象。

在Spring Boot中，Bean是一个被Spring容器管理的Java对象。当Spring容器启动时，它会读取应用程序的配置文件（如application.properties或application.yml）并创建所有的Bean。开发者可以在配置文件中定义Bean，也可以使用注解的方式定义Bean。定义Bean的方式多种多样，包括XML配置文件、Java配置类、注解等。

Spring容器中的Bean是单例的，也就是说，只会创建一个实例，并在需要的地方共享使用。当一个Bean被注入到另一个Bean中时，它实际上是将这个Bean的引用注入到另一个Bean中，而不是创建一个新的实例。

定义Bean： 1）@Component注解 2）XML配置 3） @Bean 搭配@Configration

管理Bean：1）创建和销毁 2）提供依赖项（其他Bean或属性）3）拦截对象方法调用进行AOP代理增强

### Bean的生命周期

Bean ⽣命周期的整个执⾏过程描述：

----------------------------------------创建--------------------------------------------------

1）根据配置情况调⽤ Bean 构造⽅法或⼯⼚⽅法实例化 Bean。

2）利⽤依赖注⼊完成 Bean 中所有属性值的配置注⼊。

----------------------------------------接口--------------------------------------------------

3）如果 Bean 实现了 BeanNameAware 接⼝，则 Spring 调⽤ Bean 的 setBeanName() ⽅法传⼊当前 Bean 的 id 值。

4）如果 Bean 实现了 BeanFactoryAware 接⼝，则 Spring 调⽤ setBeanFactory() ⽅法传⼊当前⼯⼚实例的引⽤。

5）如果 Bean 实现了 ApplicationContextAware 接⼝，则 Spring 调⽤ setApplicationContext() ⽅法传⼊当前 ApplicationContext 实例的引⽤。

6）如果 BeanPostProcessor 和 Bean 关联，则 Spring 将调⽤该接⼝的预初始化⽅法

postProcessBeforeInitialzation() 对 Bean 进⾏加⼯操作，此处⾮常重要，Spring 的 AOP 就是利⽤它实现的。

7）如果 Bean 实现了 InitializingBean 接⼝，则 Spring 将调⽤ afterPropertiesSet() ⽅法。

8）如果在配置⽂件中通过 init-method 属性指定了初始化⽅法，则调⽤该初始化⽅法。

9）如果 BeanPostProcessor 和 Bean 关联，则 Spring 将调⽤该接⼝的初始化⽅法 postProcessAfterInitialization()。

此时，Bean 已经可以被应⽤系统使⽤了。

---------------------------------------容器---------------------------------------------------

10）如果在 <bean> 中指定了该 Bean 的作⽤范围为 scope="singleton"，则将该 Bean 放⼊ Spring IoC 的缓存池中，将触发 Spring 对该 Bean 的⽣命周期管理；如果在 <bean> 中指定了该 Bean 的作⽤范围为 scope="prototype"，则将该 Bean 交给调⽤者，调⽤者管理该 Bean 的⽣命周期，Spring 不再管理该 Bean。

---------------------------------------销毁---------------------------------------------------

11）如果 Bean 实现了 DisposableBean 接⼝，则 Spring 会调⽤ destory() ⽅法将 Spring 中的 Bean 销毁；如果在配置⽂件中通过 destory-method 属性指定了 Bean 的销毁⽅法，则 Spring 将调⽤该⽅法对 Bean 进⾏销毁。

注意：Spring 为 Bean 提供了细致全⾯的⽣命周期过程，通过实现特定的接⼝或 <bean> 的属性设置，都可以对 Bean 的⽣命周期过程产⽣影响。

来源 [知乎](https://zhuanlan.zhihu.com/p/315303279)


## 对IoC和DI的简要理解

### 什么是控制反转 Inversion of Controll

IoC即是Spring框架中的IoC容器、将对象管理的控制权反转了，原本是对象内部直接控制，对象主动去获取、生成、销毁依赖的对象，Spring框架就是将对象依赖的控制权集成到了IoC容器中，自动管理对象的生命周期和对象间的关系。

Spring通过反射机制实现IoC。

### 什么是依赖注入Dependency Injection

DI即是由IoC容器将某个依赖关系动态地智能地添加（注入）到组件之中。DI的目的是提高组件服用率，增强系统灵活性和可扩展性。
●谁依赖于谁：当然是应用程序依赖于IoC容器；
●为什么需要依赖：应用程序需要IoC容器来提供对象需要的外部资源；
●谁注入谁：很明显是IoC容器注入应用程序某个对象，应用程序依赖的对象；
●注入了什么：就是注入某个对象所需要的外部资源（包括对象、资源、常量数据）。


DI的本质是为了提高Object的复用程度，提升效率降低消耗。例如对象之间的相互调用，可以通过在BCD对象中各自new出一个A对象来使用，但会多耗时多占空间；Spring依赖注入的优势就是可以通过生产一个Bean对象的实例，在所有需要注入的地方去调用，Spring会自动管理bean对象的创建和销毁。

### DI的实现：
@Resource和@Service配合进行对象注入实现bean的相互依赖
当我们需要定义某个类为一个bean的时候，就可以在这个类的类名上一行加一个@Service注解；
当我们需要在某个类中定义一个属性，并且该属性是一个已存在的bean，在为该属性赋值或注入的时候，就需要在该属性的上一行添加一个@Resource注解。

参考 [依赖注入的五种方式](https://blog.csdn.net/shadow_zed/article/details/72566611)


Spring注解 Annotation
@Service，用于标注业务层组件（通常定义的 Service 层就用这个注解）； 
@Controller，用于标注控制层组件（如 Struts 中的 action）；
@Repository，用于标注数据访问组件，即 DAO 层组件； 
@Component，泛指组件，当组件不好归类的时候，我们就可以用这个注解进行标注。

注解@Resource的作用相当于@Autowired，只不过@Autowired按byType自动注入，而@Resource则默认按byName自动注入。



### 对AOP和OOP的理解：
OOP基本是父类子类的纵向的树形的继承体系（也是因为java类不允许多继承），抽取对象之间的相同点形成父类，而AOP所谓的面向切面，是更细分层面的抽取机制，包括水平的抽取，例如多个流程多个类中出现了相同的子流程程序(如鉴权、性能监控、日志等等)，通过OOP的纵向继承难以解决这些代码的重复问题，而AOP即可通过抽取这些细分的事务提升复用性。
