# Spring

#### 特征

- Spring是基于Java的轻量级开发框架
- 核心技术:IoC,AOP->事务处理

#### 优点

- 简化企业级开发,提升开发效率

- 高效整合框架如Mybatis,Mybatis-plus等

#### 缺点

- 依赖反射机制,降低了运行效率
- 不支持分布式
- 配置复杂,人称配置地狱

## Ioc

#### 概念

- IoC: 控制反转,将创建对象的控制权转移到程序外部,从而降低耦合度
- IoC容器: 负责对象的创建和初始化,创建的对象称为**Bean**,**Bean默认单例**
- DI: 依赖注入,在容器中建立Bean与Bean之间的关系的过程

#### Bean

##### **创建Bean的5种方式**

```java
public class Book{
    int id;
    String name;
}
```

1. 无参构造方法,直接配置Bean

```xml
<!--
直接配置Bean的信息
id: 标识
class: 类名
-->
<bean id="book" class="SUSTech.domain.Book">
</bean>
```

2. 静态工厂,通过静态方法创建对象,配置静态工厂信息,告诉pring此工厂创建的对象是Bean,需要交付给IoC容器管理

```xml
<!--
静态工厂信息
id:标识
class:类名
factory-method:创建对象的静态方法
constructor-arg value:向该方法传递的参数
-->
<bean id="bookStaticFactory" class="SUSTech.domain.BookStaticFactory"
factory-method="getBook">
    <constructor-arg value="book1"/>
</bean>

```

3. 实例工厂,需要配置实例工厂对象,再配置

   factory-bean:指定使用哪个工厂实例
   factory-method:使用哪个工厂方法,繁琐

4. 抽象工厂(推荐)实现Spring的BeanFactory接口,并配置该实现类即可

##### **bean的生命周期**

 1.创建对象,分配内存 2.设置属性,初始化 3.用户使用bean 4. 销毁

#### DI

**依赖注入的方式**

-  setter注入:利用setter方法建立Bean之间的关系,配置文件中设置property name

- 构造器注入:在构造方法里建立Bean的关系,配置文件设置construction-arg name, name为构造器传入的形参,耦合度较高

- 自动装配:按类型,按名称,由IoC自动注入


### 注解开发

##### bean的创建

@Component 定义Bean---->@Repository表示数据层的Bean,@Service 业务层, @Controller 控制层

@ComponenScan 指定Bean扫描的范围

##### bean的生命周期控制

@PostConstruct 初始化

@PreDestroy 销毁

##### 依赖注入

@AutoWired 作用于变量, 自动装配,按类型装配,通过暴力反射找到依赖项

@Qualifier 根据Bean的名称装配,依赖于@AutoWired

@Value 基本类型注入,${}注入配置文件数据

@PropertySource 标注配置文件位置,多个配置文件使用数组格式,不能使用通配符

##### 第三方Bean管理

@Configuration 定义配置类

@Bean 自定义Bean,作用于方法,讲方法的返回值定义成Bean,容器中存在的Bean可以通过形参传入方法中使用

@Import 导入要用的配置类,导入多个配置类使用数组格式



## AOP

#### AOP相关概念

Aop:面向切面编程,无侵入式进行增强功能

连接点: 程序的任意位置,Spring中理解为方法的执行 

切入点: 匹配连接点的式子, @cutpoint(修饰符 返回值 类.方法名(参数))

通知:切入点执行的操作,即共性功能,在Spring中体现为方法

切面:描述切入点与通知的对应关系



#### AOP编写流程

1. 在pom.xml导入aspect坐标
2. 制作连接点,方法 (指要增加AOP功能的方法)
3. 制作共性功能(通知类,通知) 
   - 通知类加@component定义为Bean交给Spring容器管理
   - 通知类加@Aspect表示这个类实现了AOP功能
   - 配置类加上@EnableAspectJAutoProxy用于寻找标注了@Aspect的通知类.
4. 定义切入点,即共性功能的作用方法
   - 在通知类定义一个方法,并加@CutPoint("execution(修饰符 返回值 类.方法名(参数))"),excution内的方法即为切入点
5. 制作切面,绑定通知和切入点的关系
   - 在通知类定义一个新方法,并加@before/@around/@after...,方法体用于绑定通知和切入点的关系,

### Spring事务

#### 概念

事务作用:保证一系列数据库操作同时成功或者同时失败,类似于原子操作

Spring事务:在**数据层**或者**业务层**保证一系列操作同时成功或者同时失败

#### 应用事务流程

1. @Transactional 标注方法,将此方法定义为事务,通常写在接口上,所有实现类的相应方法都能生效

2. PlatformTransactionManager Spring的平台事务管理器接口,需要传入数据源,执行数据源的事务管理

3. 在配置类加@EnableTransactionManager,开启事务管理

#### 事务的属性设置

@Transactional()默认遇到error和runtime exception才会回滚,需要修改rollbackFor属性,

例如@Transactional(rollbackFor={IOException.class})

@Transactional()的propagation属性可以定义事物的传播行为



*事务管理员指被@Transactional()标注的方法,事务协调员指被管理员管理的方法

```java
/*@Transactional()
void A(){
B();
C();
D();
}
void B(){};
void C(){};
@Transactional(propagation=Propagation.REQUIRES_NEW)
void D(){};*/
//D的事务传播行为设置了REQUIRES_NEW,在A创建事务时D会创建一个新的事务,不与B,C同时回滚.
```



