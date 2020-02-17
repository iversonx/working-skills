# 问答-HandlerMapping

> 注意：以下内容是基于注解配置进行解答

## 问1：HandlerMapping的作用

`HandlerMapping`是SpringMVC的核心接口之一，其作用是根据`HttpServletRequest`来获取相应的`Handler`。`HandlerMapping`只有一个`getHandler`方法用于返回`Handler`的执行链(`Handler`以及所有的`HandlerInterceptor`)。

简而言之，`HandlerMapping`就是用于请求与处理程序之间的映射。

## 问2：HandlerMapping的初始化过程

SpringMVC默认定义了5个`HandlerMapping`类型的bean：

- `RequestMappingHandlerMapping`

- name为viewControllerHandlerMapping的`HandlerMapping`

- name为beanNameHandlerMapping的`BeanNameUrlHandlerMapping`

- name为resourceHandlerMapping的`HandlerMapping`

- name为defaultServletHandlerMapping的`HandlerMapping`

除`RequestMappingHandlerMapping`外，其他4个HandlerMapping bean主要是为了兼容旧版本的`Controller`编写模式。

因此这里以`RequestMappingHandlerMapping`为例：

1. Spring容器在初始化时，`WebMvcConfigurationSupport#requestMappingHandlerMapping`方法会被执行；

2. 这个方法会创建一个`RequestMappingHandlerMapping`的实例，并对属性进行初始化；

3. 当属性都设置完成之后，会去扫描容器中所有的Bean，当判断某个Bean是`Controller`时，就根据该Bean中每个被`@RequestMapping`注解标注的方法都创建一个`RequestMappingInfo`实例；

4. 然后将这些`RequestMappingInfo`注册到`RequestMappingHandlerMapping`的`MappingRegistry`属性中。SpringMVC使用`MappingRegistry`来维护处理方法的映射。

以上内容可阅读：

-  `WebMvcConfigurationSupport#requestMappingHandlerMapping`

- `WebMvcConfigurationSupport#viewControllerHandlerMapping`

- `WebMvcConfigurationSupport#beanNameHandlerMapping`

- `WebMvcConfigurationSupport#resourceHandlerMapping`

- `WebMvcConfigurationSupport#defaultServletHandlerMapping`

- `RequestMappingHandlerMapping#afterPropertiesSet`

## 问3：DispatcherServlet如何从HandlerMapping中获取Handler

`DispatcherServlet`并不是直接依赖`Handler`，而是依赖`HandlerExecutionChain`，`HandlerExecutionChain`表示`Handler`的执行链，包括`Handler`和各种拦截器。

1. `DispatcherServlet`在初始化时，会从`ApplicationContext`中获取所有的`HandlerMapping`的实现，并设置`DispatcherServlet`的`handlerMappings`集合属性中。

2. `DispatcherServlet`接收到请求时，会遍历`handlerMappings`，逐个调用`getHandler`方法

3. `HandlerMapping`首先根据`HttpServletRequest`获取uri，并根据uri获取`handler`；需要注意的是此时的`Handler`可能是一个`beanName`，如果是beanName，会根据beanName到容器中获取对应实例，并重新赋值给`handler`属性；

4. 最终将`handler`封装到`HandlerExecutionChain`中，并返回`HandlerExecutionChain`。

以上内容具体可以阅读源码:

- `DispatcherServlet#getHandler`

- `AbstractHandlerMapping#getHandler`

- `AbstractHandlerMethodMapping#getHandlerInternal`

## 问4：HandlerMapping是否可以扩展

当需要自定义`HandlerMapping`时，可以直接实现`HandlerMapping`接口；也可以继承`HandlerMapping`的抽象实现；又或者继承上面3个具体实现，然后重写方法来完成自定义。


