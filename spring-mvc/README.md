# 带着问题阅读SpringMVC源码

Spring MVC是Spring基于MVC(model-view-controller)架构的web框架。Spring MVC是围绕`DispatcherServlet`设计的，核心组件有：`DispatcherServlet`,`HandlerAdapter`,`HandlerMethod`,`Controller`,`ModelAndView`,`ViewResolver`,`HandlerMapping`。

- DispatcherServlet：处理HTTP请求的中央控制器，调用其他组件来处理请求。

- HandlerMapping：处理器映射器，将请求URL与Handler进行映射。

- HandlerAdapter：处理器适配器，帮助DispatcherServlet调用映射到请求的Handler。

- HandlerMethodArgumentResolver: 方法参数解析

- HandlerMethod：处理器方法，封装Handler的方法信息。

- ViewResolver：视图解析器，将基于字符串的逻辑视图名称解析真实的视图。

- Handler：处理器，通常为开发者定义的Controller。

## Spring MVC处理请求的流程

![流程图](processing-request-flow.png)

1. `DispatcherServlet`接收到客户端请求后，首先根据uri到`HandlerMapping`中确定`Handler`；此时`DispatcherServlet`会得到`HandlerExecutionChain`实例；

2. 接着`DispatcherServlet`会根据`Handler`确定`HandlerAdapter`；

3. `HandlerAdapter`解析方法参数并调用`Handler`，进行请求处理；根据`Handler`的返回内容得到`ModelAndView`；并将`ModelAndView`返回给`DispatcherServlet`；

4. `DispatcherServlet`调用`ViewResolvers`对`ModelAndView`进行解析获得实际的`View`，接着对view进行渲染。最后返回view；

5. `DispatcherServlet`将view响应给客户端进行展示。

以下内容是基于注解方式配置进行解答：
- [HandlerMapping相关问题](HandlerMapping.md)
- [HandlerAdapter相关问题](HandlerAdapter.md)
