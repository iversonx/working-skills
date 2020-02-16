# HandlerMapping

`HandlerMapping`是SpringMVC的核心接口之一，主要用于`HttpServletRequest`与`Handler`之间的映射。

`HandlerMapping`只有一个`getHandler`方法用于获取`HandlerExecutionChain`。`HandlerExecutionChain`包含了`Handler`以及所有的`HandlerInterceptor`。

排除被`@Deprecated`标记的实现类后，HandlerMapping有3个具体的实现类

## SimpleUrlHandlerMapping

实现从URL映射到Handler，支持映射到bean实例和映射到bean名称。

> 如果handler的scope不是单例，应该使用映射到bean名称，否则会一直使用同一个实例处理请求

SimpleUrlHandlerMapping使用HashMap来定义URL与handler的映射关系。

### 使用示例

```java
@Configuration
@ComponentScan("")
public class WebMvcConfig extends DelegatingWebMvcConfiguration {

    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        registry.jsp().prefix("/").suffix(".jsp");
    }

    @Autowired
    private SimpleController simpleController;
    @Bean
    public SimpleUrlHandlerMapping simpleUrlHandlerMapping() {
        SimpleUrlHandlerMapping mapping = new SimpleUrlHandlerMapping();
        Map<String, Object> map = new HashMap<>();
        // 映射bean名称

        map.put("/simple", "simpleController");
        // 映射bean实例

        map.put("/simple2", simpleController);
        mapping.setUrlMap(map);
        return mapping;
    }
}
```

## BeanNameUrlHandlerMapping

从URL映射到名称以/开头的bean。

### 使用示例

SpringMVC默认创建了`BeanNameUrlHandlerMapping`的实例，则只需编写Handler即可。

```java
@Component("/beanName")
public class BeanNameController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView mav = new ModelAndView();
        mav.setViewName("simple");
        mav.addObject("simpleName", "BeanNameController");
        return mav;
    }

}
```

当请求/beanName时，BeanNameController对请求进行处理。

## RequestMappingHandlerMapping

根据`@Controller`类的`@RequestMapping`注解和方法级别的`@RequestMapping`注解创建RequestMappingInfo实例。

使用`@Controller`加`@RequestMapping`是编写handler最常用的方式。

> RequestMappingInfo定义了请求和Handler方法之间的映射

### 使用示例

```java
@Controller
@RequestMapping("/example")
public class ExampleController {

    @RequestMapping("/show")
    public String show() {
        return "show";
    }    
}
```
