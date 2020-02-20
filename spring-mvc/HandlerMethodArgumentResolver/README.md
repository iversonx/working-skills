# HandlerMethodArgumentResolver

`HandlerMethodArgumentResolver`用于在给定请求中将方法参数解析为参数值。

在调用具体的handler之前，会调用`HandlerMethodArgumentResolver`进行参数解析。

`HandlerMethodArgumentResolver`是一个策略接口，定义了两个方法

- `boolean supportsParameter(MethodParameter parameter)`
  
    返回当前解析器是否支持给定的方法参数

- `Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,  NativeWebRequest webRequest, WebDataBinderFactory binderFactory)`
  
    将方法参数解析为参数值

`HandlerMethodArgumentResolver`有十几种策略实现。这里介绍常见的几种：

- `RequestParamMethodArgumentResolver`：解析使用`@RequestParam`注解的方法参数、文件上传相关的方法参数以及没有被任何注解标记的方法参数。

- `PathVariableMethodArgumentResolver`：解析使用`@PathVariable`注解的方法参数。

- `RequestHeaderMethodArgumentResolver`：解析使用`@RequestHeader`注解的方法参数(参数类型为Map除外)。

- `RequestResponseBodyMethodProcessor`：通过使用`HttpMessageConverter`读写请求或响应的主体，解析以`@RequestBody`注解的方法参数，并处理以`@RequestBody`注释的方法返回值。
