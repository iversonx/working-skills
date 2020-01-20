# 问答-HandlerAdapter
## 问1：SpringMVC在什么时候创建HandlerAdapter？

`DispatcherServlet`通过`HandlerAdapter`访问`Handler`，这样`DispatcherServlet`就无需关注具体类型的`Handler`。

`HandlerAdapter`的创建时机与`HandlerMapping`一样。

具体内容可阅读:

`WebMvcConfigurationSupport#requestMappingHandlerAdapter`方法、

`RequestMappingHandlerAdapter#afterPropertiesSet`方法。

## 问2：DispatcherServlet为什么要调用HandlerAdapter，而不是直接调用Handler
