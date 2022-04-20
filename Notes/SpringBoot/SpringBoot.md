### SpringBoot

1. 注解

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

   @Import({ Lj.class })

   1. 直接导入一个指定组件到另一个组件内

   2. 属性

      value()

   3. 默认bean名为全限定类名

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

   