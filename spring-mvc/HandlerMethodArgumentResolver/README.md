# HandlerMethodArgumentResolver

`HandlerMethodArgumentResolver`用于在给定请求中将方法参数解析为参数值。

在调用具体的handler之前，会调用`HandlerMethodArgumentResolver`进行参数解析。

`HandlerMethodArgumentResolver`是一个策略接口，定义了两个方法

- `boolean supportsParameter(MethodParameter parameter)`

    返回当前解析器是否支持给定的方法参数

- `Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,  NativeWebRequest webRequest, WebDataBinderFactory binderFactory)`

    将方法参数解析为参数值

`HandlerMethodArgumentResolver`有十几种策略实现。这里介绍常见的几种：

- `RequestParamMethodArgumentResolver`
  
  用于解析使用`@RequestParam`注解的方法参数、文件上传相关的方法参数以及没有被任何注解标记的方法参数。

- `PathVariableMethodArgumentResolver`



- `RequestHeaderMethodArgumentResolver`



- `RequestResponseBodyMethodProcessor`
