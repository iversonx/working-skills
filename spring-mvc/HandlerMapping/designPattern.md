# 模板方法模式

![类层次结构](class-hierarchy.png)

从`HandlerMapping`的类层次结构可以看出所有实现类都继承自`AbstractHandlerMapping`。

## AbstractHandlerMapping

`AbstractHandlerMapping`是`HandlerMapping`的抽象实现，其定义了根据Handler以及配置的拦截器来生成`HandlerExecutionChain`的逻辑，但将根据request获取Handler的逻辑交由子类实现。

```java
public final HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {

        Object handler = getHandlerInternal(request);
        // 省略其他代码

        HandlerExecutionChain executionChain = getHandlerExecutionChain(handler, request);
        // 省略其他代码
        return executionChain;
    }

protected abstract Object getHandlerInternal(HttpServletRequest request) throws Exception;
```

从上述代码可以看出，`AbstractHandlerMapping`定义了生成`HandlerExecutionChain`的算法结构，而如何获取Handler，则由子类来实现。

看到这里突然想到模板方法的定义：定义算法结构，将某些步骤交由子类实现。由此可见SpringMVC采用了模板方法来设计`HandlerMapping`。

`AbstractHandlerMapping`相当于模板基类，它有两个直接子类：`AbstractHandlerMethodMapping`和`AbstractUrlHandlerMapping`。这两个子类实现了`AbstractHandlerMapping`的抽象方法`getHandlerInternal`。

- AbstractHandlerMethodMapping：抽象类，定义请求和HandlerMethod之间的映射。也就是将请求与handler方法进行映射
  
  ```java
  protected HandlerMethod getHandlerInternal(HttpServletRequest request) throws Exception {
  		String lookupPath = getUrlPathHelper().getLookupPathForRequest(request);
  		this.mappingRegistry.acquireReadLock();
  		try {
  			HandlerMethod handlerMethod = lookupHandlerMethod(lookupPath, request);
  			return (handlerMethod != null ? handlerMethod.createWithResolvedBean() : null);
  		}
  		finally {
  			this.mappingRegistry.releaseReadLock();
  		}
  	}
  ```

- AbstractUrlHandlerMapping：抽象类，定义请求与beanName或bean实例进行绑定
  
  ```java
  protected Object getHandlerInternal(HttpServletRequest request) throws Exception {
  		String lookupPath = getUrlPathHelper().getLookupPathForRequest(request);
  		Object handler = lookupHandler(lookupPath, request);
  		if (handler == null) {
  			Object rawHandler = null;
  			if ("/".equals(lookupPath)) {
  				rawHandler = getRootHandler();
  			}
  			if (rawHandler == null) {
  				rawHandler = getDefaultHandler();
  			}
  			if (rawHandler != null) {
  				// Bean name or resolved handler?
  				if (rawHandler instanceof String) {
  					String handlerName = (String) rawHandler;
  					rawHandler = getApplicationContext().getBean(handlerName);
  				}
  				validateHandler(rawHandler, request);
  				handler = buildPathExposingHandler(rawHandler, lookupPath, lookupPath, null);
  			}
  		}
  		return handler;
  	}
  ```
