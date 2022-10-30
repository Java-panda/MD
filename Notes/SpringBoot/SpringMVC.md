SpringMVC

- 主线流程

  - ![img](C:\Users\77023\Desktop\MD笔记\MD\Notes\SpringBoot\images\SpringMVC流程.svg)

  - ![img](C:\Users\77023\Desktop\MD笔记\MD\Notes\SpringBoot\images\SpringMVC时序.svg)

- DispatcherServlet:前端控制器
  - 负责调度分发请求
- Handler
  - 使用@Controller+@RequestMapping(最常用)
  - 实现Controller接口
  - 实现HttpRequestHandler
- HandlerMapping:处理器映射器
  - RequestMappingHandlerMapping
  - SimpleUrlHandlerMapping
  - BeanNameUrlHandlerMapping
  - RouterFunctionHandlerMapping
  - WelcomePageHandlerMapping
- HandlerAdapter:处理器适配器
  - RequestMappingHandlerAdapter
  - SimpleControllerHandlerAdapter
  - HttpRequestHandlerAdapter
  - HandlerFunctionAdapter
- ViewResolver:视图解析器
- 视图渲染
  - JSP
  - FreeMaker
  - Thymeleaf
- 九大组件
  - 容器初始化时执行DispatcherServlet.initStrategies()初始化组件
    - HandlerMapping:映射请求URL和处理器
    - HandlerAdapter:适配不同的处理器
    - HandlerExceptionResolver:处理器执行异常时的解析器
    - ViewResolver:视图解析器将通过viewName返回一个View
    - RequestToViewNameTranslator:从请求中获取viewName
    - LocaleResolver:区域解析器,国际化等
    - ThemeResolver:主题解析器
    - MultipartResolver:文件解析器
    - FlashMapManager:重定向相关