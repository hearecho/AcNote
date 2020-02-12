# java基础知识

# java容器

### hashmap

1. 阈值 为 大小 * 0.75
2. 大于阈值之后变进行扩容
3. 

# Java并发

### 多线程三个特性

#### 原子性

操作不可分割，可以使用synchronized关键字，和lock锁可以实现操作的原子性，另外AtomicInteger也具有原子性

#### 可见性

一个线程对于共享变量的操作对其他线程是可见的，可以使用volatile 关键字定义变量；

#### 顺序性

防止java代码优化重拍，volatile 关键字定义变量可以实现；

### 线程池

关键参数：

| 参数名称      | 作用                                               | 设置依据 |
| ------------- | -------------------------------------------------- | -------- |
| corePoolSzie  | 线程池核心线程数量                                 |          |
| mixPoolSize   | 线程池最大线程数量                                 |          |
| taskQueue     | 储存任务队列（同步）                               |          |
| keepAliveTime | 非核心线程存活时间                                 |          |
| threadFactory | 生成线程的工厂类                                   |          |
| handler       | 任务队列满与线程数达到最大数量时间，执行的拒绝策略 |          |



# java虚拟机

### java内存模型

#### 运行时数据区域

![](img/javavm.png)

### java内存管理

#### Mirror GC 和 FULL GC

1. mirror gc  新生代空间不足就会发生一次，速度快，回收新生代
2. full gc：回收老年代和新生代，老年代对象其存活时间长，因此 Full GC 很少执行，执行速度会比 Minor GC 慢很多。

#### 内存分配策略

1. 对象优先进入新生代
2. 大对象直接进入老年代
3. 长期存活的对象进入老年代，经过Mirror GC还存活的对象年龄加一，动态对象年龄判定
4. 空间分配担保

#### FULL GC的触发条件

1. System.gc()调用
2. 空间担保失败
3. 老年代空间不足

### GC

#### 垃圾收集器

1. Serial收集器
2. ParNew收集器
3. Parallel Scavenge 收集器
4. Serial Old收集器
5. Parallel Old收集器
6. CMS收集器
7. G1收集器

#### 垃圾收集方法

1. 标记-清除

> 将可以收集的对象进行标记，之后回收即可，速度快，但是会遗留很多内存碎片，导致无法分配大对象；

2. 标记-整理

> 将可以手机的对象进行标记，回收之后，将未收集对象进行整理，速度慢，但是不会有碎片；

3. 复制

> 将内存划分为大小相等的两块，每次只使用其中一块，当这一块内存用完了就将还存活的对象复制到另一块上面，然后再把使用过的内存空间进行一次清理。
>
> 在的商业虚拟机都采用这种收集算法回收新生代，但是并不是划分为大小相等的两块，而是一块较大的 Eden 空间和两块较小的 Survivor 空间，每次使用 Eden 和其中一块 Survivor。在回收时，将 Eden 和 Survivor 中还存活着的对象全部复制到另一块 Survivor 上，最后清理 Eden 和使用过的那一块 Survivor

4. 分代收集

> 新生代使用：复制算法
>
> 老年代使用：标记 - 清除 或者 标记 - 整理 算法

### java内存泄漏定位方法命令

1. jstat



# java框架

### Spring

#### AOP(面向切面编程)

##### 静态代理

##### 动态代理

1. java自带的代理，主要用于代理一个实现接口的类时，使用java自带的代理

> 主要涉及java.lang.reflect中的两个类，Proxy 和 InvocationHandler 
> InvocationHandler是一个接口，通过实现该接口定义横切逻辑，并通过反射机制调用目标类的代码，动态将横切逻辑和业务逻辑编辑在一起。只能为实现接口的类创建代理;
>
> 除了public之外的其他所有方法都不能进行代理，缺省也不行，**public static**也不行

1. cglib代理，除去上述方式其他的使用这个进行代理

> 是一个强大的高性能，高质量的代码生成类库，可以在运行期扩展Java类与实现Java接口，Cglib封装了asm,可以在运行期动态生成新的class。可以是普通类，也可以是实现接口的类
>
> 由于其通过生成目标类子类的方式来增强，因此**不能被子类继承的方法都不能被增强**，private、static、final 方法

#### IOC(控制反转) 

#### DI(依赖注入)

#### SpringMVC处理请求的流程

1. 客户端请求到DispatcherServlet
2. DispatcherServlet根据请求地址查询映射处理器HandleMapping，获取Handler
3. 请求HandlerAdatper执行Handler
4. 执行相应的Controller方法，执行完毕返回ModelAndView
5. 通过ViewResolver解析视图，返回View
6. 渲染视图，将Model数据转换为Response响应
7. 将结果返回给客户端

#### bean周期

1. Bean实例的创建
2. 为Bean实例设置属性
3. 调用Bean的初始化方法（这个过程中aop在postprocessor中进行aop代理，返回一个增强后的bean）
4. 应用可以通过IOC容器使用Bean
5. 当容器关闭时，调用Bean的销毁方法

### mybaits

### springboot

#### 自动配置

#### 注解的原理和实现







