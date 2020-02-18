# 问答-HandlerAdapter

## 问1：HandlerAdapter初始化过程

Spring MVC默认定义了3个`HandlerAdapter`类型的Bean。并在容器初始化时实例化这3个`HandlerAdapter`：

- `RequestMappingHandlerAdapter`：支持通过`@ReuqestMapping`定义的具有方法参数和返回类型签名的`HandlerMethod`类型的handler

- `HttpRequestHandlerAdapter`：支持`HttpRequestHandler`类型的handler

- `SimpleControllerHandlerAdapter`：支持`Controller`类型的handler

以`RequestMappingHandlerAdapter`为例，

首先会创建`RequestMappingHandlerAdapter`的实例，并设置属性。`RequestMappingHandlerAdapter`在初始化完成后，还会再次检查`argumentResolvers`,`returnValueHandlers`是否null；如果为null，则设置相应的默认值，以便`RequestMappingHandlerAdapter`可以进行参数解析和返回值处理。

以上内容看阅读源码

- `WebMvcConfigurationSupport#requestMappingHandlerAdapter()`,

- `WebMvcConfigurationSupport#httpRequestHandlerAdapter()`,

- `WebMvcConfigurationSupport#simpleControllerHandlerAdapter()`,

- `RequestMappingHandlerAdapter`

## 问2：为什么不直接使用Handler，而使用HandlerAdapter

在SpingMVC中处理请求的handler被声明为Object类型，因此handler可以任何类型。

`HandlerAdapter`的目的就是适配不同类型handler，每种类型handler都有一个与之对应的`HandlerAdapter`实现；这样`DispatcherServlet`无需包含任何特定的handler的代码，只需与`HandlerAdapter`接口进行交互，从而使`DispatcherServlet`可以无限扩展。


