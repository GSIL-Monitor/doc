####Spring
  * Spring一般基于下面三种方案进行服务或者功能暴漏
    * xml  <beans>
    * 注解  @Bean
    * Java    


 * Bean生命周期
   * Spring Bean的生命周期简单易懂。在一个bean实例被初始化时，需要执行一系列的初始化操作以达到可用的状态。同样的，当一个bean不在被调用时需要进行相关的析构操作，并从bean容器中移除。 Bean的生命周期由两组回调（call back）方法组成。
      * 初始化之后调用的回调方法。
      * 销毁之前调用的回调方法。
      * 
   * 了解Bean的生命周期就是方便在bean初始化或者销毁时进行个性化操作。Spring框架提供了以下四种方式来管理bean的生命周期事件：

       * InitializingBean和DisposableBean回调接口
       * 针对特殊行为的其他Aware接口
       * Bean配置文件中的Custom init()方法和destroy()方法
       * @PostConstruct和@PreDestroy注解方式  