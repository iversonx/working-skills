# 阅读SpringMVC源码

Spring MVC是Spring基于MVC(model-view-controller)架构的web框架。

## Spring MVC处理请求的流程

![流程图](processing-request-flow-2.png)

1. 客户端发起一个HTTP请求到服务器，DispatcherServlet处理这个请求；

2. DispatcherServlet首先从根据url从HandlerMapping中获取包含HandlerMethod的执行链；

3. 接着DispatcherServlet根据HandlerMethod来确定具体的HandlerAdapter；

4. DispatcherServlet调用HandlerAdapter来处理请求；

5. HandlerAdapter调用HandlerMethodArgumentResolver对Handler的方法参数进行解析绑定；

6. 参数绑定后，HandlerAdapter调用具体的Handler处理请求；

7. HandlerAdapter根据Handler的方法返回值确定视图信息，然后将视图信息封装在ModelAndView中，并返回ModelAndView；

8. DispatcherServlet根据ModelAndView确定具体的ViewResolver；

9. DispatcherServlet调用ViewResolver来解析ModelAndView来得到View；

10. View进行视图渲染，最后将视图展示给客户端。

## 组件

- [DispatcherServlet](DispatcherServlet.md)：处理HTTP请求的中央控制器，调用其他组件来处理请求

- [HandlerMapping](HandlerMapping.md)：处理器映射器，将请求URL与Handler进行映射

- [HandlerMethod](HandlerMethod.md)：处理器方法，封装Handler的方法信息

- [HandlerAdapter](HandlerAdapter.md)：处理器适配器，调用具体的Handler来处理请求

- [HandlerMethodArgumentResolver](HandlerMethodArgumentResolver.md)：用于解析请求参数，然后传递给Handler方法

- [HandlerMethodReturnValueHandler](HandlerMethodReturnValueHandler.md)：用于处理Handler方法的返回值

- [ViewResolver](ViewResolver.md)：视图解析器，将基于字符串的逻辑视图名称解析真实的视图

- [ModelAndView](ModelAndView.md)：包含响应数据和真实视图信息

- [View](View.md)：真实视图

## 问答

以下内容是基于注解方式配置进行解答：

- [问答-HandlerMapping](QA-HandlerMapping.md)
- [问答-HandlerAdapter](QA-HandlerAdapter.md)
- [问答-SpringMVC如何解析方法参数](QA-SpringMVC如何解析方法参数.md)
