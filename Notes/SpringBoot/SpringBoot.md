### SpringBoot

1. 脉络

   1. IOC
      1. XML👉BeanDefinitionReader👉BeanDefinition👉BeanFactoryProcessor👉BeanPostProcessor:实例化前👉实例化👉BeanPostProcessor:实例化后👉依赖注入👉BeanPostProcessor:初始化前👉初始化👉BeanPostProcessor:初始化后
   2. AOP
      1. ObjenesisCglibAopProxy
      2. JdkDynamicAopProxy
   3. R
   4. AbstractApplicationContext.refresh()
      1. 
   5. 常用接口
      1. Environment:

2. 注解

   @SpringBootApplication

   1. SpringBoot启动类,同时默认扫面范围为启动类及其子包下

   2. 属性

      exclude()

      1. ​	排除某些组件的自动装配

      excludeName()
      scanBasePackages()
      scanBasePackageClasses()
      nameGenerator()
      proxyBeanMethods()

   3. 包含以下三个注解

      @SpringBootConfiguration
      @EnableAutoConfiguration
      @ComponentScan

   @Configuration

   1. 声明这是一个配置类,会被IOC当做一个组件读入

   2. 属性

      value()
      proxyBeanMethods()

      1. true:组件单例模式
      2. false:组件多例模式

   3. 内部使用@Bean标注方法

   4. @Configuration本身也是组件,需要在扫描范围下

   @Bean

   1. 向IOC加入一个bean对象实例

   2. 属性

      value()
      name()

      1. ​	定义bean名,默认为方法名

      autowire()
      autowireCandidate()
      initMethod()
      destroyMethod()

   @ComponentScan

   1. 定义组件扫描路径

   2. 属性

      value()
      basePackages()
      basePackageClasses()
      nameGenerator()
      scopeResolver()
      scopedProxy()
      resourcePattern()
      useDefaultFilters()
      includeFilters()
      excludeFilters()
      lazyInit()

   @Import({ com.panda.external.Partner.class })

   1. 直接导入一个第三方组件或者非扫描包下的组件或者扫描包下的非Spring注解声明组件,简单的理解就是手动向IOC容器里面添加指定类的实例对象

   2. 属性

      String[] value()

   3. 默认bean名为全限定类名com.panda.external.Partner

   4. Import的其他用法

      1. Import({ xxxImportSelecter.class })

         1. 自定义ImportSelecter接口动态实现value的数组值,xxxImportSelecter.class本身不会被注入

         2. ``` java
            public class MyImportSelecter implements ImportSelector {
                @Override
                public String[] selectImports(AnnotationMetadata importingClassMetadata) {
                    //选择要导入的class数组
                    return new String[]{"com.panda.SpringBoot.bean.InnerPartner"};
                }
            }
            ```

      2. Import({ xxxImportBeanDefiniationRegister.class })

         1. 自定义ImportBeanDefiniationRegister接口把BeanDefiniation注册进容器中,xxxImportBeanDefiniationRegister.class本身不会被注入,相当于跳过String解析成BeanDefiniation的这个过程,直接告诉Spring,Bean的定义信息,可以自定义名字生成器

         2. ``` java
            public class MyImportBeanDefinitionRegister implements ImportBeanDefinitionRegistrar {
                @Override
                public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry, BeanNameGenerator importBeanNameGenerator) {
                    GenericBeanDefinition genericBeanDefinition = new GenericBeanDefinition();
                    genericBeanDefinition.setBeanClassName("com.panda.SpringBoot.bean.InnerPartner");
                    String beanName = importBeanNameGenerator.generateBeanName(genericBeanDefinition, registry);
                    registry.registerBeanDefinition(beanName,genericBeanDefinition);
                }
            }
            ```

         3. 


   @ConfigurationProperties(prefix = "com.panda.lj")

   1. 配置绑定

   2. 属性

      value()
      prefix()
      ignoreInvalidFields()
      ignoreUnknownFields()

   @PropertySource(value = { "com/panda/springboot/config/lj.properties" })

   @Conditional

   1. @ConditionalOnProperty
   2. @ConditionalOnResource
   3. @ConditionalOnBean
   4. @ConditionalOnMissingBean
   5. @ConditionalOnClass
   6. @ConditionalOnMissingClass
   7. @ConditionalOnExpression
   8. @ConditionalOnSingleCandidate
   9. @ConditionalOnJndi
   10. @ConditionalOnJava
   11. @ConditionalOnWebApplication
   12. @ConditionalOnNotWebApplication
   13. @ConditionalOnCloudPlatform

   @ImportResource

   1. 导入xml配置文件文件,使用在组件类上

   2. 属性

      value()
      locations()
      reader()

   @Component

   1. 通用组件标识

   @Controller

   1. Controller组件标识

   @Service

   1. Service组件标识

   @Repository

   1. Repository组件标识

   导入自定义Bean

   1. 批量映射

      @Component
      @PropertySource(value = { "com/panda/springboot/config/lj.properties" })
      @ConfigurationProperties(prefix = "com.panda.lj")

   2. 单个映射

      @Component
      @PropertySource(value = { "com/panda/springboot/config/artifact.properties" })
      	@Value(value = "${com.panda.artifact.artifactName}")

3. Springboot中获取ApplicationContext的两种方式

   1. 在其他组件中直接使用Autowire自动注入

      1. ``` Java
         @Autowired
         private ApplicationContext ac;
         ```

      2. 

   2. 使用自定义类实现ApplicationContextAware接口,实现setApplicationContext()方法

      1. ```java
         @Component
         public class MyApplicationContext implements ApplicationContextAware {
             private ApplicationContext applicationContext;
             @Override
             public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
                 System.out.println("接口回调获取ApplicationContext");
                 System.out.println(applicationContext);
                 this.applicationContext=applicationContext;
             }
         
             public ApplicationContext getApplicationContext() {
                 return applicationContext;
             }
         }
         
         ```

      2. 

4. BeanFactory和FactoryBean

   1. BeanFactory是IOC容器是Spring的容器顶层接口

   2. FactoryBean是一个工厂类的对象

      1. ``` java
         @Component
         public class UserFactoryBean implements FactoryBean {
             @Override
             public Object getObject() throws Exception {
                 User user = new User();
                 user.setName("FactoryBean");
                 return user;
             }
             @Override
             public Class<?> getObjectType() {
                 return User.class;
             }
             @Override
             public boolean isSingleton() {
                 return FactoryBean.super.isSingleton();
             }
         }
         ```

      2. 默认User对象的name是userFactoryBean,UserFactoryBean对象的name是&userFactoryBean

5. ApplicationContext/BeanFactory

   1. ![image-20221022204040924](C:\Users\77023\Desktop\MD笔记\MD\Notes\SpringBoot\images\image-20221022204040924.png)

   2. 接口

      1. MessageSource:获取国际化消息
         1. getMessage("login.tip",null, Locale.US)
      2. ResourcePatternResolver:读取类路径文件资源
         1. getResources("classpath*:META-INF/spring.factories")
      3. EnvironmentCapable:环境变量相关
         1. getEnvironment().getProperty("JAVA_HOME")
      4. ApplicationEventPublisher:解耦组件

      ``` java
      //事件对象
      public class MyEvent extends ApplicationEvent {
          /**
           * Create a new {@code ApplicationEvent}.
           *
           * @param source the object on which the event initially occurred or with
           *               which the event is associated (never {@code null})
           */
          public MyEvent(Object source) {
              super(source);
          }
      }
      
      //监听器
      @Component
      public class MyEventListener {
          @EventListener
          public void listenMyEvent(MyEvent myEvent){
              System.out.println("listenMyEvent");
          }
      }
      
      ac.publishEvent(new MyEvent(ac)
      ```

   3. 实现

      1. ClassPathXmlApplicationContext
         1. 基于类路径XML文件创建AC
      2. FileSystemXmlApplicationContext
         1. 基于磁盘路径XML文件创建AC
      3. AnnotationConfigApplicationContext
         1. 基于注解创建AC
      4. AnnotationConfigServletWebServerApplicationContext
         1. 基于Servlet的Web应用

6. Factory

   1. BeanFactory:容器根接口
   2. MetadataReaderFactory:类的元数据读取工厂(Resource,ClassMetadata,AnnotationMetadata)

7. BeanFactoryPostProcessor

   1. ConfigurationClassPostProcessor:@ComponentScan,@Bean
   2. 

8. BeanPostProcessor

   1. AutowiredAnnotationBeanPostProcessor:@Autowired,@Value
   2. CommonAnnotationBeanPostProcessor:@Resource,@PostConstruct,@PreDestroy
   3. InstantiationAwareBeanPostProcessor
      1. postProcessBeforeInitialization:实例化前
      2. postProcessAfterInitialization:实例化后
      3. postProcessBeforeInstantiation:初始化前
      4. postProcessAfterInstantiation:初始化后
         1. True:继续依赖注入
         2. False:跳过依赖注入
      5. postProcessProperties:依赖注入
   4. DestructionAwareBeanPostProcessor
      1. postProcessBeforeDestruction:销毁前
   5. ConfigurationPropertiesBindingPostProcessor:@ConfigurationProperties

9. Resolver

   1. ContextAnnotationAutowireCandidateResolver:@value
   2. StandardEnvironment.resolvePlaceholders():${}

10. AOP

11. 动态代理

12. @Configuration

13. AOP

14. @Lazy

15. Transaction

    1. 使用方法:
       1. 配置类上添加@EnableTransactionManagement注解开启允许管理事务
       2. 在业务类或者业务方法上添加@Transactional注解开启管理事务
    2. 事务失效:

16. SpringBoot启动过程及Bean的创建生命周期

    1. 扫描包获取class
    2. 读取class
    3. 推断构造器
       1. 只有一个则默认使用
       2. 如果有多个构造器,默认使用无参构造,如果没有无参构造则报错
    4. 实例化对象
    5. 依赖注入(@Autowire)
       1. 先通过类型再通过名字查找,没有则创建
    6. 初始化前(@PostConstruct)
    7. 初始化
       1. 
    8. 初始化后
       1. AOP
    9. 销毁(@PreDestroy)

17. 循环依赖

    1. 场景:
       1. A依赖B,B也依赖A
    2. 解决方式:三级缓存,@Lazy(直接懒加载依赖注入类,用的时候才创建,避免循环依赖)
       1. 第一级缓存(存完整Bean对象)
          1. singletonObjects
       2. 第二级缓存(存提前AOP对象,保证AOP对象也是单例的)
          1. earlySingletonObjects
       3. 第三级缓存(存原始对象target打破循环)
          1. singletonFactories
    3. 原理:
       1. A类实例化a放入三级缓存Map<name,lambda<a>>同时放入一个Set<A>中标记为正在创建
       2. 依赖注入B类
          1. B实例化b
          2. 依赖注入A类
             1. 一级缓存中有吗?→正在创建吗?→发现循环依赖需要A→二级缓存中有吗?→三级缓存中取出a→AOP→pa→pa放入二级缓存并移除三级缓存中的a
          3. 其他初始化...
       3. 依赖注入C类
          1. C实例化c
          2. 依赖注入A类
             1. 一级缓存中有吗?→正在创建吗?→发现循环依赖需要A→二级缓存中有吗?(第二步已经有了所以直接返回)
          3. 其他初始化...
       4. 其他初始化...
       5. 最后把二级缓存中的A取出放入一级缓存并移除二级缓存

18. 执行顺序相关

    1. 构造函数 --> @PostConstruct --> InitializingBean接口 --> @Bean init-method 

    