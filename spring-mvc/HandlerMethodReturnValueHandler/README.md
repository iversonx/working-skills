# HandlerMethodReturnValueHandler

`HandlerMethodReturnValueHandler`用于处理handler方法调用的返回值。

`HandlerMethodReturnValueHandler`定义了两个方法：

- `boolean supportsReturnType(MethodParameter returnType)`：当前处理器是否支持给定的方法返回类型

- `void handleReturnValue(Object returnValue, MethodParameter returnType, ModelAndViewContainer mavContainer, NativeWebRequest webRequest) `：通过想Model添加属性并设置view或将`ModelAndViewContainer.setRequestHandled(boolean)`设置为true来处理给定返回值，以指示响应已被直接处理。

`HandlerMethodReturnValueHandler`有多个实现，这里列举几个常见的实现：

- `ModelAndViewMethodReturnValueHandler`：处理类型为`ModelAndView`的返回值。

- `ViewnameMethodReturnValueHandler`：处理类型为`void`和`String`的返回值。

- `ViewMethodReturnValueHandler`：处理类型为`View`的返回值。
