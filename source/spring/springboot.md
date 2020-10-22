spring-boot-starter-xxx是spring官方提供的starter,xxx-spring-boot-starter是第三方提供给spring的starter.

starter机制帮助我们完成了项目启动所需的相关jar包，

#### spring-boot是如何完成bean的配置的
传统的springmvc项目中bean都在相应的xml文件中进行配置，而starter通过引入相应的autoconfigure包，并通过注解（@Configuration @Bean)来创建一个基于java的配置类，并以此来代替xml配置文件。

@Configuration:注解的类可以看作是能生产让IOC容器管理的Bean实例的工厂
@Bean：注解告诉spring,一个带Bean注解的方法将返回一个对象，该对象应该被注册到IOC容器中，

以mybatis-spring-boot-starter为例，通过MybatisAutoConfiguration这个类，自动帮我们生成了SqlSessionFactory这些Mybatis的重要实例，并交给spring的容器去管理，从而完成Bean的自动注册。


sprin-boot的注解

+ @ConditionalOnBean，仅在当前上下文中存在某个bean时，才会实例化这个Bean。

+ @ConditionalOnClass，某个class位于类路径上，才会实例化这个Bean。

+ @ConditionalOnExpression，当表达式为true的时候，才会实例化这个Bean。

+ @ConditionalOnMissingBean，仅在当前上下文中不存在某个bean时，才会实例化这个Bean。

+ @ConditionalOnMissingClass，某个class在类路径上不存在的时候，才会实例化这个Bean。

+ @ConditionalOnNotWebApplication，不是web应用时才会实例化这个Bean。

+ @AutoConfigureAfter，在某个bean完成自动配置后实例化这个bean。

+ @AutoConfigureBefore，在某个bean完成自动配置前实例化这个bean

spring-boot提供了DataSourceAutoConfiguration这个类，该类在spring-boot-autoconfigure.jar中，这个包自动帮我们配置了很多好，如：jdbc、kafka、loggin、mail、mongo等，当然这些配置呢也只有在引入相应的jar包之后才会生效。


Bean如何获取参数

@ConfigurationProperties 注解的作用是把yml或者properties配置文件转化为Bean
@EnableConfigurationProperties:使@ConfigurationProperties注解生效。
但这些Bean又是如何被发现和加载的呢？

@SpringBootApplication注解内容：

@Configuration的作用上面我们已经知道了，被注解的类将成为一个bean配置类。

@ComponentScan的作用就是自动扫描并加载符合条件的组件，比如@Component和@Repository等，最终将这些bean定义加载到spring容器中。

@EnableAutoConfiguration 这个注解的功能很重要，借助@Import的支持，收集和注册依赖包中相关的bean定义。

通过反射机制将spring.factories中@Configuration类实例化为对应的java实列

如果要让一个普通类交给Spring容器管理，通常有以下方法：

1、使用 @Configuration与@Bean 注解

2、使用@Controller @Service @Repository @Component 注解标注该类，然后启用@ComponentScan自动扫描

3、使用@Import 方法(springboot使用了这种方式)


自动配置的关键几步以及相应的注解总结如下：

1、@Configuration&与@Bean->基于java代码的bean配置

2、@Conditional->设置自动配置条件依赖

3、@EnableConfigurationProperties与@ConfigurationProperties->读取配置文件转换为bean。

4、@EnableAutoConfiguration、@AutoConfigurationPackage 与@Import->实现bean发现与加载。