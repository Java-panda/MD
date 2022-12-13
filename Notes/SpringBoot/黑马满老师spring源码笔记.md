1. ApplicationContext

   1. 继承的接口

      1. BeanFactory

         1. getBean()

      2. MessageSource:获取国际化消息

         1. getMessage("login.tip",null, Locale.US)

      3. ResourceLoader:读取类路径文件资源

         1. getResources("classpath*:META-INF/spring.factories")

      4. EnvironmentCapable:环境变量相关

         1. getEnvironment().getProperty("JAVA_HOME")

      5. ApplicationEventPublisher:解耦组件

         ```java
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
         
         User user = new User();
         ac.publishEvent(new MyEvent(user)
         ```

   2. 常用实现

      1. ClassPathXmlApplicationContext
         1. 基于类路径XML文件创建AC
      2. FileSystemXmlApplicationContext
         1. 基于磁盘路径XML文件创建AC
      3. AnnotationConfigApplicationContext
         1. 基于注解创建AC
      4. AnnotationConfigServletWebServerApplicationContext
         1. 基于Servlet的Web应用

