# 问答-HandlerAdapter

## 问1：HandlerAdapter初始化过程

Spring MVC默认定义了3个`HandlerAdapter`类型的Bean。并在容器初始化时实例化这3个`HandlerAdapter`：

- `RequestMappingHandlerAdapter`：支持通过`@ReuqestMapping`定义的具有方法参数和返回类型签名的`HandlerMethod`类型的handler

- `HttpRequestHandlerAdapter`：支持`HttpRequestHandler`类型的handler

- `SimpleControllerHandlerAdapter`：支持`Controller`类型的handler

以`RequestMappingHandlerAdapter`为例，`RequestMappingHandlerAdapter`在初始化完成后，还会再次检查`argumentResolvers`,`returnValueHandlers`是否null；如果为null，则设置相应的默认值，以便`RequestMappingHandlerAdapter`可以进行参数解析和返回值处理。

以上内容看阅读源码：

`WebMvcConfigurationSupport#requestMappingHandlerAdapter()`,

`WebMvcConfigurationSupport#httpRequestHandlerAdapter()`,

`WebMvcConfigurationSupport#simpleControllerHandlerAdapter()`,

`RequestMappingHandlerAdapter`

## 问3：DispatcherServlet为什么不直接调用Handler，而是调用HandlerAdapter

`DispatcherServlet`接收请求时，会根据请求uri从`HandlerMapping`中获取`Handler`的执行链`HandlerExecutionChain`，`HandlerExecutionChain`中已经包含了`Handler`。此时已经可以获取具体`Handler`了。

SpringMVC使用`HandlerAdapter`的目的为了让`DispatcherServlet`与`Handler`解耦，这样`DispatcherServlet`不需要知道`Handler`的具体类型，就可以完成请求；从而使用`Handler`可以很好的进行扩展。
