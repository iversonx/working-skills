# SpringMVC如何解析方法参数？

`DispatcherServlet`处理请求时会调用`HandlerAdapter`来处理请求；`HandlerAdapter`最终会创建一个`HandlerMethod`实例来进行参数解析和调用handler处理请求。具体代码在`InvocableHandlerMethod#invokeForRequest`方法中。

`invokeForRequest`方法首先进行方法参数的解析绑定，然后通过反射调用handler的方法，并将参数传递过去。

## 方法参数解析

1.  首先从`HandlerMethod`中获取Handler方法参数数组，SpringMVC使用`MethodParameter`来存储方法参数信息，因此得到是`MethodParameter`类型的数组。
    
2.  如果Handler没有定义方法参数，即方法参数数组的length为0，则不进行参数解析，直接调用handler进行请求处理；
    
3.  如果定义了方法参数，则开始遍历数组，逐个进行参数解析。
    
4.  在解析某个参数前，都会先获取合适方法参数解析器`HandlerMethodArgumentResolver`。如果没有找到合适解析器，将抛出`IllegalStateException`异常。

> HandlerMethodArgumentResolver拥有多种解析策略，例如RequestParamArgumentResolver用于对使用@RequestParam注解的参数或没有使用任何注解的参数进行解析
