# java基础知识

# java容器

https://blog.csdn.net/xzp_12345/article/details/79251174

### hashmap

1. 阈值 为 大小 * 0.75
2. 大于阈值之后变进行扩容

### hashtable，hashmap，concurenthashmap的区别



# Java并发

### 多线程三个特性

#### 原子性

操作不可分割，可以使用synchronized关键字，和lock锁可以实现操作的原子性，另外AtomicInteger也具有原子性

#### 可见性

一个线程对于共享变量的操作对其他线程是可见的，可以使用volatile 关键字定义变量；

#### 顺序性

防止java代码优化重拍，volatile 关键字定义变量可以实现；

### 线程池

##### 关键参数：

| 参数名称      | 作用                                               | 设置依据 |
| ------------- | -------------------------------------------------- | -------- |
| corePoolSzie  | 线程池核心线程数量                                 |          |
| mixPoolSize   | 线程池最大线程数量                                 |          |
| taskQueue     | 储存任务队列（同步）                               |          |
| keepAliveTime | 非核心线程存活时间                                 |          |
| threadFactory | 生成线程的工厂类                                   |          |
| handler       | 任务队列满与线程数达到最大数量时间，执行的拒绝策略 |          |

##### 自带的线程池

1. newFixedThreadPool(固定大小的线程池)
2. newSingleThreadExecutor(单线程的线程池)
3. newScheduledThreadPool(线程池中的线程可以定时执行一些任务)
4. newCachedThreadPool(可缓存的线程池)，如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。

### volatile关键字

##### 实现原理

- read、load、use动作必须**连续出现**。
- assign、store、write动作必须**连续出现**

所以volatile变量能够保证读取前必须先从主内存刷新最新的值，写入后必须立即同步回主内存；

# java虚拟机

### java内存模型

#### 运行时数据区域

![](../../img/javavm.png)

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

### java内存泄露以及内存溢出

##### 内存泄漏：

> java分配一个对象之后，这个对象丢失，没有被回收，这就造成了内存泄漏，内存泄漏最后便会造成内存溢出；

###### 分类

1. 常发性内存泄漏。发生内存泄漏的代码会被多次执行到，每次被执行的时候都会导致一块内存泄漏。 

2. 偶发性内存泄漏。发生内存泄漏的代码只有在某些特定环境或操作过程下才会发生。常发性和偶发性是相对的。对于特定的环境，偶发性的也许就变成了常发性的。所以测试环境和测试方法对检测内存泄漏至关重要。 

3. 一次性内存泄漏。发生内存泄漏的代码只会被执行一次，或者由于算法上的缺陷，导致总会有一块仅且一块内存发生泄漏。比如，在类的构造函数中分配内存，在析构函数中却没有释放该内存，所以内存泄漏只会发生一次。 

4. 隐式内存泄漏。程序在运行过程中不停的分配内存，但是直到结束的时候才释放内存。严格的说这里并没有发生内存泄漏，因为最终程序释放了所有申请的内存。但是对于一个服务器程序，需要运行几天，几周甚至几个月，不及时释放内存也可能导致最终耗尽系统的所有内存。所以，我们称这类内存泄漏为隐式内存泄漏。 

###### 内存泄漏检查命令

1. jstat

##### 内存溢出(out of memory)：

> 指程序在申请内存时，没有足够的内存空间供其使用，出现out of memory

###### 引发原因

1. 内存中加载的数据量过于庞大，如一次从数据库取出过多数据；

2. 集合类中有对对象的引用，使用完后未清空，使得JVM不能回收；

3. 代码中存在死循环或循环产生过多重复的对象实体；

4. 使用的第三方软件中的BUG；

5. 启动参数内存值设定的过小



### java内存读写

lock：作用于主内存，把变量标识为线程独占状态。

unlock：作用于主内存，解除独占状态。

read：作用主内存，把一个变量的值从主内存传输到线程的工作内存。

load：作用于工作内存，把read操作传过来的变量值放入工作内存的变量副本中。

use：作用工作内存，把工作内存当中的一个变量值传给执行引擎。

assign：作用工作内存，把一个从执行引擎接收到的值赋值给工作内存的变量。

store：作用于工作内存的变量，把工作内存的一个变量的值传送到主内存中。

write：作用于主内存的变量，把store操作传来的变量的值放入主内存的变量中。

# javaIO

### java字节流

> 装饰器模式实现的

#### 关闭方式

1. 一个流一个流的关闭,从外到内顺序关闭；先打开的后关闭，后打开的先关闭
2. 可以只调用外层流的close方法

```java
FileOutputStream fos = new FileOutputStream("d:\\a.txt");
OutputStreamWriter osw = new OutputStreamWriter(fos, "UTF-8");
BufferedWriter bw = new BufferedWriter(osw);
bw.write("java IO close test");
bw.close();
```



#### FileOutStream/FileInputStream

#### reader/writer

```java
public static void readFileContent(String filePath) throws IOException {

    FileReader fileReader = new FileReader(filePath);
    BufferedReader bufferedReader = new BufferedReader(fileReader);

    String line;
    while ((line = bufferedReader.readLine()) != null) {
        System.out.println(line);
    }

    // 装饰者模式使得 BufferedReader 组合了一个 Reader 对象
    // 在调用 BufferedReader 的 close() 方法时会去调用 Reader 的 close() 方法
    // 因此只要一个 close() 调用即可
    bufferedReader.close();
}
```



### java NIO

#### 主要类

##### buffer类

> 从内部结构上来看，Buffer就像一个数组，它可以保存多个类型相同的数据，Buffer是一个抽象类，对应的基本数据类型都有相应的Buffer类：CharBuffer、ShortBuffer、IntBuffer、LongBuffer、FloatBuffer、DoubleBuffer,可以在底层的字节数组上进行get/set操作。

1. 构造方法

```java
static XxxBuffer allocate(int capacity)
//创建一个容量为capacity的XxxBuffer对象。
```

2. 简单示例

```java
public class bufferStu {
    public static void main(String[] args) {
        CharBuffer buff = CharBuffer.allocate(8);
        System.out.println("cap: "+buff.capacity());
        System.out.println("limit: "+buff.limit());
        System.out.println("position: "+buff.position());
        // 放入元素
        buff.put('a');
        buff.put('b');
        buff.put('c');
        int a = buff.position();
        System.out.println("加入三个元素后，position = " + buff.position());
        System.out.println("使用get()取出元素" + buff.get(a-1));
        // 调用flip()方法，将position设为0，limit设为position的位置 可以看作反转buffer
        buff.flip();
        System.out.println("执行flip()后，limit = " + buff.limit());
        System.out.println("position = " + buff.position());
        // 取出第一个元素
        System.out.println("第一个元素（position=0）：" + buff.get());
        System.out.println("取出一个元素后，position = " + buff.position());
        // 调用clear方法，将position设为0，limit设为capacity的位置
        buff.clear(); // 7
        System.out.println("执行clear()后，limit = " + buff.limit());
        System.out.println("执行clear()后，position = " + buff.position());
        System.out.println("执行clear()后，缓冲区内容并没有被清除：" + buff.get(2));
        System.out.println("执行绝对读取后，position = " + buff.position());
    }
}
//position 在不执行flip函数时，是得到的最后一个元素所在位置+1，执行flip函数之后，便可以通过get函数按次序取出，
```

##### Channel类

> Channel类似于传统的流对象，但与传统的流对象有两个主要区别。
>
>    ①Channel可以直接将指定文件的部分或全部直接映射成Buffer.
>
>    ②程序不能直接访问Channel中的数据，包括读取、写入都不行，Channel只能与Buffer进行交互

1. 分类

①支持线程之间通信：Pipe.SinkChannel、Pipe.SourceChannel

 ②支持TCP网络通信：ServerSocketChannel、SocketChannel

 ③支持UDP网络通信：DatagramChannel

 ④支持文件之间通信：FileChannel

2. 获取

所有的Channel都不应该通过构造器直接创建，而是通过传统节点的getChannel()方法来返回对应的Channel，不同的节点流获得的Channel不一样，例如FileInputStream的getChannel()方法返回的是FileChannel.

3. 示例代码

```java
public class FileChannelTest {
	public static void main(String[] args) {
		FileChannel inChannel = null;
		FileChannel outChannel = null;
		try {
			File f = new File("FileChannelTest.java");
			// 创建FileInputStream，以该文件输入流创建FileChannel
			inChannel = new FileInputStream(f).getChannel();
			// 将FileChannel里的全部数据映射成ByteBuffer
			// MapMode：映射模式，只读和读写两种模式
			// 0,f.length()：将Channel对应索引的数据映射到buffer中
			MappedByteBuffer buffer = inChannel.map(
					FileChannel.MapMode.READ_ONLY, 0, f.length());
			// 使用GBK的字符集来创建解码器
			Charset charset = Charset.forName("GBK");
			// 以文件输出流创建FileBuffer，用以控制输出
			outChannel = new FileOutputStream("a.txt").getChannel();
			// 直接将buffer里的数据全部输出
			outChannel.write(buffer);
			// 再次调用buffer的clear()方法，复原limit、position的位置
			buffer.clear();
			// 创建解码器(CharsetDecoder)对象
			CharsetDecoder decoder = charset.newDecoder();
			// 使用解码器将ByteBuffer转换成CharBuffer
			CharBuffer charBuffer = decoder.decode(buffer);
			// CharBuffer的toString方法可以获取对应的字符串
			System.out.println(charBuffer);
		} catch (IOException ex) {
			ex.printStackTrace();
		} finally {
			try {
				if (inChannel != null)
					inChannel.close();
				if (outChannel != null)
					outChannel.close();
			} catch (IOException ex) {
				ex.printStackTrace();
			}
		}
	}
}
```

##### Charset类

> java默认使用Unicode字符集，但很多操作系统并不是用Unicode字符集，那么当从系统中读取数据到java程序中时，就可能出现乱码等问题。JDK1.4提供了Charset来处理字节序列和字符序列（字符串）之间的转换关系，该类包含了用于创建解码器和编码器的方法。

```java
public class CharsetTransform {
	public static void main(String[] args) throws Exception {
		// 创建简体中文对应的Charset
		Charset cn = Charset.forName("GBK");
		// 获取cn对象对应的编码器和解码器
		CharsetEncoder cnEncoder = cn.newEncoder();
		CharsetDecoder cnDecoder = cn.newDecoder();
		// 创建一个CharBuffer对象
		CharBuffer cbuff = CharBuffer.allocate(3);
		cbuff.put('孙');
		cbuff.put('悟');
		cbuff.put('空');
		cbuff.flip();
		// 将CharBuffer中的字符序列转换成字节序列
		ByteBuffer bbuff = cnEncoder.encode(cbuff);
		// 循环访问ByteBuffer中的每个字节
		for (int i = 0; i < bbuff.capacity(); i++) {
			System.out.print(bbuff.get(i) + " ");
		}
		
		// 将ByteBuffer的数据解码成字符序列
		System.out.println("\n" + cnDecoder.decode(bbuff));
	}
}
```



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

##### 五种依赖注入的方式

1. @Autowired(required注入)，可以将这个注解加在构造器，字段，方法上都可以；@Autowired默认是根据参数类型进行自动装配，且必须有一个Bean候选者注入默认required=true，如果允许出现0个Bean候选者需要设置属性“required=false”，“required”属性含义和@Required一样，只是@Required只适用于基于XML配置的setter注入方式,只能打在setting方法上。
2. setter方法注入；这是最简单的注入方式，假设有一个SpringAction，类中需要实例化一个SpringDao对象，那么就可以定义一个private的SpringDao成员变量，然后创建SpringDao的set方法（这是ioc的注入入口）
3. 构造器注入；这种方式的注入是指带有参数的构造函数注入，我创建了两个成员变量SpringDao和User，但是并未设置对象的set方法，所以就不能支持第一种注入方式，这里的注入方式是在SpringAction的构造函数中注入，也就是说在创建SpringAction对象时要将SpringDao和User两个参数值传进来
4. 静态工厂的方法注入；
5. 实例工厂的方法注入

##### 如果在一个系统中有很多不同包下的bean名字是一样的，怎么解决注入时的冲突问题？

xml配置直接修改bean的名称即可，注解设置相应的bean名称

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







