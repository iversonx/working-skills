# 问答-HandlerMapping

## 序：HandlerMapping的作用

`HandlerMapping`是SpringMVC的核心接口之一，其作用是根据`HttpServletRequest`来获取相应的`Handler`。`HandlerMapping`只有一个`getHandler`方法用于返回`Handler`的执行链(`Handler`以及所有的`HandlerInterceptor`)。

简而言之，`HandlerMapping`就是用于请求与处理程序之间的映射。

## 问1：SpringMVC在什么时候创建HandlerMapping？

容器初始化时，会创建并初始化`HandlerMapping`实例，默认会创建5种`HandlerMapping`的实现实例：

- `RequestMappingHandlerMapping`，并设置优先级为0；

- 基于`ViewControllerRegistry`的`SimpleUrlHandlerMapping`；

- `BeanNameUrlHandlerMapping`，并设置优先级为2；

- 基于`ResourceHandlerRegistry`的`SimpleUrlHandlerMapping`；

- 基于`DefaultServletHandlerConfigurer`的`SimpleUrlHandlerMapping`。

以`RequestMappingHandlerMapping`为例：

`RequestMappingHandlerMapping`的所有bean属性都设置完成之后，会去扫描容器中所有的Bean，当判断某个Bean是`Controller`时，就根据该Bean中每个被`@RequestMapping`注解标注的方法都创建一个`RequestMappingInfo`实例；然后将这些`RequestMappingInfo`注册到`RequestMappingHandlerMapping`的`MappingRegistry`属性中。SpringMVC使用`MappingRegistry`来维护处理方法的映射。

以上内容可阅读：`WebMvcConfigurationSupport#requestMappingHandlerMapping`方法、`RequestMappingHandlerMapping#afterPropertiesSet`方法。

## 问2：DispatcherServlet如何从HandlerMapping中获取Handler？

`DispatcherServlet`并不是直接依赖`Handler`，而是依赖`HandlerExecutionChain`，`HandlerExecutionChain`表示`Handler`的执行链，包括`Handler`和各种拦截器。

`DispatcherServlet`在初始化时，会从`ApplicationContext`中获取所有的`HandlerMapping`的实现，并设置`DispatcherServlet`的`handlerMappings`集合属性中。

2. `DispatcherServlet`接收到请求时，会遍历`handlerMappings`；

3. 接着调用`HandlerMapping#getHandler`方法；

4. `HandlerMapping`首先根据`HttpServletRequest`获取uri，并根据uri获取`HandlerMethod`实例，`HandlerMethod`中包含一个`Object`类型的`handler`；需要注意的是此时的`Handler`可能是一个`beanName`，如果是beanName，会根据beanName到容器中获取对应实例，并重新赋值给`handler`属性。

5. 最终将`handler`封装到`HandlerExecutionChain`中，并返回`HandlerExecutionChain`。

以上内容具体可以阅读:

`DispatcherServlet#getHandler`方法。

## 问3：HandlerMapping的实现，以及是否可以扩展HandlerMapping？

首先从`HandlerMapping`的类层次结构上来看有3个具体实现类(排除被标记为`@Deprecated`)

- SimpleUrlHandlerMapping：直接将URL与Handler映射的实现；通过`LinkedHashMap`来存储url与Handler的映射关系。

```properties
# 示例
/welcome.html=ticketController
/show.html=ticketController
```

- BeanNameUrlHandlerMapping：将url映射到以名称斜线(/)开头的bean。

- RequestMappingHandlerMapping：`RequestMappingHandlerMapping`将`@Controller`类中被`@RequestMapping`标注的方法与url进行映射。

当需要自定义`HandlerMapping`时，可以直接实现`HandlerMapping`接口；也可以继承`HandlerMapping`的抽象实现；又或者继承上面3个具体实现，然后重写方法来完成自定义。
