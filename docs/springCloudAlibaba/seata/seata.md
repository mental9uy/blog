#seata
##全局异常处理Seata事务失效解决方案
​ 最近的项目用到了seata来管理全局事务，在进行测试的时候，发现当service A 调用Service B时，如果ServiceA报错，ServiceB能回滚，但是如果ServiceB报错，ServiceA是无法进行回滚的。

​ 经过查找，发现是因为当时为了系统能统一处理全局异常，返回统一的异常信息给前端做了一个全局异常拦截，具体代码如下
```java
/**
 * Controller统一异常处理
 *
 * @author : 777666
 * @date : 2022/01/19
 */
@ControllerAdvice
public class AllControllerAdvice {
    private static Logger logger = LoggerFactory.getLogger(AllControllerAdvice.class);

    /**
     * 应用到所有@RequestMapping注解方法，在其执行之前初始化数据绑定器
     */
    @InitBinder
    public void initBinder(WebDataBinder binder) {
    }

    /**
     * 把值绑定到Model中，使全局@RequestMapping可以获取到该值
     */
    @ModelAttribute
    public void addAttributes(Model model) {
    }


    /**
     * 捕捉BusinessException自定义抛出的异常
     *
     * @return
     */
    @ResponseStatus(HttpStatus.OK)
    @ExceptionHandler(value = BusinessException.class)
    @ResponseBody
    public ResultData handleBusinessException(BusinessException e) {
        String message = e.getMessage();
        if (message.indexOf(":--:") > 0) {
            String[] split = message.split(":--:");
            return ResultData.error(split[0], split[1]);
        }
        return ResultData.error(CodeEnum.DATA_ERROR.getCode(), message, null);
    }


    @ResponseStatus(HttpStatus.OK)
    @ExceptionHandler(value = HttpMessageNotReadableException.class)
    @ResponseBody
    public ResultData handleHttpMessageNotReadableException(HttpMessageNotReadableException e) {
        logger.info("参数错误：" + e.getMessage());
        return ResultData.error(CodeEnum.PARAM_ERROR.getCode(), e.getMessage(), null);
    }

    @ResponseStatus(HttpStatus.OK)
    @ExceptionHandler(value = MethodArgumentNotValidException.class)
    @ResponseBody
    public ResultData handleMethodArgumentNotValidException(MethodArgumentNotValidException e) {
        logger.info("参数错误：" + e.getMessage());
        return ResultData.error(CodeEnum.PARAM_ERROR.getCode(), e.getBindingResult().getAllErrors().get(0).getDefaultMessage(), null);
    }

    /**
     * 全局异常捕捉处理
     */
    @ResponseBody
    @ExceptionHandler(value = Exception.class)
    @ResponseStatus(HttpStatus.OK)
    public ResultData<String> errorHandler(Exception ex) {
        logger.error("接口出现严重异常：{}", ex.getMessage());
        return ResultData.error(CodeEnum.ERROR.getMsg());
    }

}
```

这么做的一个好处就是，无论你代码里面报了什么异常，我这个AOP都可以拦截到，并且返回给前端一个约定的值，而不会说返回给前端一串英文异常信息。

那么在使用seata的时候，如果这么做会有什么问题呢？

我们使用ServiceA调用ServiceB，此时让ServiceB 抛出一个异常，会发现一个现象，ServiceB回滚了，ServiceA没回滚，那么此时可以说分布式事务失效了。

经过查阅资料发现，如果异常被我们内部处理了（我们自定义的全局异常AOP拦截了），seata不会再处理这个异常信息，导致ServiceA的事务回滚失败。

明白了这个，问题就比较好解决了

要么进行手动回滚，要么让所有分布式事务请求的接口不被这个AOP拦截不就可以了吗

针对AOP的处理，seata官方有一个解决方案

在这里，我们不用这个方式

首先，我们看下@ControllerAdvice这个注解



在这个注解中有一个basePackages的属性，也就是说，如果配置了这个属性，将只会拦截basePackages指定的包下面发生的异常

所以，我们只需要指定basePackages的值，再把远程调用的所有服务抽出来，关键代码如下

全局异常拦截器只需要处理正常controller下的所有请求



此时远程调用的包结构如下



此时再试一下，在ServiceB抛出一个除零异常


ServiceA此时感知到后，进行Rollback



说一说这种方式的局限性

在ServiceA和ServiceB都配置了全局异常的前提下，ServiceA去调用ServiceB

​ 如果ServiceA排除了远程调用的feign远程包，ServiceB没排除，那么ServiceB的分布式异常将被内部自己处理，seata不再处理，所以ServiceA不会回滚。

有一点就是，无论ServiceA、ServiceB是否排除feign远程包，只要作为调用方的ServiceA出现异常，作为被调用方ServiceB的事务都将发生回滚（由于对seata的内部原理还不是很清楚，具体情况需要后续研究了才明白）。

总结：在使用了seata管理分布式事务的情况下，如果项目内部使用了全局异常处理，需要把远程调用的包排除出去，否则可能会导致分布式事务失效
