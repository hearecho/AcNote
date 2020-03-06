# java基础知识

### 阻塞队列

几种常用的阻塞队列

- ArrayBlockingQueue ：一个由数组结构组成的有界阻塞队列。
- LinkedBlockingQueue ：一个由链表结构组成的有界阻塞队列。
- PriorityBlockingQueue ：一个支持优先级排序的无界阻塞队列。
- DelayQueue：一个使用优先级队列实现的无界阻塞队列。
- SynchronousQueue：一个不存储元素的阻塞队列。
- LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。
- LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列。

### Java程序启动至少启动几个线程

2个或者五个

### 抽象类和接口区别，以及各自的使用场景

1、相同点
     A. 两者都是抽象类，都不能实例化。
     B. interface实现类及abstrct class的子类都必须要实现已经声明的抽象方法。

2.、不同点
     A. interface需要实现，要用implements，而abstract class需要继承，要用extends。
     B. 一个类可以实现多个interface，但一个类只能继承一个abstract class。
     C. interface强调特定功能的实现，而abstract class强调所属关系。 
    D. 尽管interface实现类及abstrct class的子类都必须要实现相应的抽象方法，但实现的形式不同。interface中的每一个方法都是抽象方法，都只是声明的(declaration, 没有方法体)，实现类必须要实现。而abstract class的子类可以有选择地实现。
     这个选择有两点含义：
            一是Abastract class中并非所有的方法都是抽象的，只有那些冠有abstract的方法才是抽象的，子类必须实现。那些没有abstract的方法，在Abstrct class中必须定义方法体。
            二是abstract class的子类在继承它时，对非抽象方法既可以直接继承，也可以覆盖；而对抽象方法，可以选择实现，也可以通过再次声明其方法为抽象的方式，无需实现，留给其子类来实现，但此类必须也声明为抽象类。既是抽象类，当然也不能实例化。
    E. abstract class是interface与Class的中介。
         interface是完全抽象的，只能声明方法，而且只能声明pulic的方法，不能声明private及protected的方法，不能定义方法体，也不能声明实例变量。然而，interface却可以声明常量变量，并且在JDK中不难找出这种例子。但将常量变量放在interface中违背了其作为接口的作用而存在的宗旨，也混淆了interface与类的不同价值。如果的确需要，可以将其放在相应的abstract class或Class中。
       abstract class在interface及Class中起到了承上启下的作用。一方面，abstract class是抽象的，可以声明抽象方法，以规范子类必须实现的功能；另一方面，它又可以定义缺省的方法体，供子类直接使用或覆盖。另外，它还可以定义自己的实例变量，以供子类通过继承来使用。

3、interface的应用场合
     A. 类与类之前需要特定的接口进行协调，而不在乎其如何实现。
     B. 作为能够实现特定功能的标识存在，也可以是什么接口方法都没有的纯粹标识。
     C. 需要将一组类视为单一的类，而调用者只通过接口来与这组类发生联系。
     D. 需要实现特定的多项功能，而这些功能之间可能完全没有任何联系。

4、abstract class的应用场合
      一句话，在既需要统一的接口，又需要实例变量或缺省的方法的情况下，就可以使用它。最常见的有：
      A. 定义了一组接口，但又不想强迫每个实现类都必须实现所有的接口。可以用abstract class定义一组方法体，甚至可以是空方法体，然后由子类选择自己所感兴趣的方法来覆盖。
      B. 某些场合下，只靠纯粹的接口不能满足类与类之间的协调，还必需类中表示状态的变量来区别不同的关系。abstract的中介作用可以很好地满足这一点。
      C. 规范了一组相互协调的方法，其中一些方法是共同的，与状态无关的，可以共享的，无需子类分别实现；而另一些方法却需要各个子类根据自己特定的状态来实现特定的功能

### 反射使用场景,反射原理

反射的原理

> 每个类都有一个 **Class** 对象，包含了与类有关的信息。当编译一个新类时，会产生一个同名的 .class 文件，该文件内容保存着 Class 对象。

> 类加载相当于 Class 对象的加载，类在第一次使用时才动态加载到 JVM 中。也可以使用 `Class.forName("com.mysql.jdbc.Driver")` 这种方式来控制类的加载，该方法会返回一个 Class 对象。

> 反射可以提供运行时的类信息，并且这个类可以在运行时才加载进来，甚至在编译时期该类的 .class 不存在也可以加载进来。

反射的使用场景

1. 临时创建一个类
2. 访问一个类的私有属性

### ThreadLocal

http://www.jasongj.com/java/threadlocal/

#### 作用

> ThreadLocal是每个线程独有的，所以不是用来共享的，ThreadLocal 适用于每个线程需要自己独立的实例且该实例需要在多个方法中被使用，也即变量在线程间隔离而在方法或类间共享的场景

### List删除元素，fast-fail fast-safe

https://blog.csdn.net/ch717828/article/details/46892051

#### List删除元素

> 使用iterator.remove()方法，不能使用list.remove(),不然会报错；ConcurrentModificationException异常。

```java
List<Integer>  list = new ArrayList<>();
//        List<Integer> linked = new LinkedList<>();
for (int i = 0; i <5 ; i++) {
    list.add(i);
}
Iterator<Integer> iterator= list.iterator();
while (iterator.hasNext()) {
    int i = iterator.next();
    if (i==3){
        iterator.remove();
    }
    System.out.println(i);
}
for (int i:list) {
    System.out.println(i);
}
```

### 泛型以及泛型擦除。List<A>类型的list,可以加入无继承关系的B类型对象吗？如何加入？

https://blog.csdn.net/briblue/article/details/76736356

泛型

```java
public class Cache<T> {
	T value;

	public Object getValue() {
		return value;
	}

	public void setValue(T value) {
		this.value = value;
	}
    //泛型方法
    public  <E> E testMethod1(E e){
		return e;
	}
}
//T  属于类型参数
```

1. 普通的 Object 代替一切类型这样简单粗暴而言，泛型使得数据的类别可以像参数一样由外部传递进来。它提供了一种扩展能力。它更符合面向抽象开发的软件编程宗旨。
2. 当具体的类型确定后，泛型又提供了一种类型检测的机制，只有相匹配的数据才能正常的赋值，否则编译器就不通过。所以说，它是一种类型安全检测机制，一定程度上提高了软件的安全性防止出现低级的失误。
3. 泛型提高了程序代码的可读性，不必要等到运行的时候才去强制转换，在定义或者实例化阶段，因为 Cache<String>这个类型显化的效果，程序员能够一目了然猜测出代码要操作的数据类型。

三种通配符

1. <?>无限定的通配符:由于类型是未知的，所以不能使用增删改的功能，但是可以查
2. <? extend T>有上限的通配符，代表类型 T 及 T 的子类，仍然不具有写的能力
3. <? super T>有下限的通配符，代表类型 T 及 T 的超类，具有写的能力

类型擦除

> 在泛型类被类型擦除的时候，之前泛型类中的类型参数部分如果没有指定上限，如``<T extends String>`` 则会被转译成普通的 Object 类型，如果指定了上限如 则类型参数就被替换成类型上限。

List<A>类型的list,可以加入无继承关系的B类型对象吗？如何加入？

> 适用类型擦除加上反射，虽然类型擦除会抹掉很多继承相关的特性，但是通过类型擦除，可以将两个毫不相关的类加入List

```java
public class ToolTest {


	public static void main(String[] args) {
		List<Integer> ls = new ArrayList<>();
		ls.add(23);
//		ls.add("text");
		try {
			Method method = ls.getClass().getDeclaredMethod("add",Object.class);
				
			method.invoke(ls,"test");
			method.invoke(ls,42.9f);
		} catch (Exception e) {
			e.printStackTrace();
		}
		for ( Object o: ls){
			System.out.println(o);
		}
	}
}
/**
23
test
42.9
**/
```



### 反序列化失败的场景

1. 没有加serialVersionUID，之后类的字段发生了改变，那么反序列化便会失败；
2. 写入文件之后（持久化）之后，类的名称发生了改变，则发序列化失败；

### java多态实现原理

> 多态的底层实现是动态绑定，即在运行时间才把方法调用和方法实现关联起来；多态的实现过程，就是方法调用动态分派的过程，通过栈帧的信息去找到被调用方法的具体实现，然后使用这个具体实现的直接引用完成方法调用。

JVM 的方法调用指令有五个，分别是：

> invokestatic 和 invokespecial 用于静态绑定，invokevirtual 和 invokeinterface 用于动态绑定。可以看出，动态绑定主要应用于虚方法和接口方法。

| 指令            | 用途                                                         |
| --------------- | ------------------------------------------------------------ |
| invokestatic    | 调用静态方法                                                 |
| invokespecial   | 调用实例构造器<init>方法、私有方法和父类方法；               |
| invokevirtual   | 调用虚方法                                                   |
| invokeinterface | 调用接口方法，运行时确定具体实现；                           |
| invokedynamic   | 运行时动态解析所引用的方法，然后再执行，用于支持动态类型语言。 |

过程:(以invokevirtual 指令为例)

1. 先从操作栈中找到对象的实际类型 class；
2. 找到 class 中与被调用方法签名相同的方法，如果有访问权限就返回这个方法的直接引用，如果没有访问权限就报错 java.lang.IllegalAccessError ；
3. 如果第 2 步找不到相符的方法，就去搜索 class 的父类，按照继承关系自下而上依次执行第 2 步的操作；
4. 如果第 3 步找不到相符的方法，就报错 java.lang.AbstractMethodError ；

### static关键字和final关键字使用情况，一个类不能被继承，除了final关键字之外，还有什么方法

static

> 表示一个方法或者一个类，或者一个属性是属于这个类，而不是属于对象；是通过类调用的；

final

> 被final修饰的方法不可以被重写，修饰的类不可以被继承，修饰的属性不可以被修改(对于基本类型，final值不变，引用类型，引用不会改变，但是引用内部的值是可以改变的)；接口的属性均是public  final

一个类不能被继承，除了final关键字之外，还有什么方法

> 将父类的构造函数设置未private，这样子类不可以调用；

存在继承的情况下，初始化顺序为：

- 父类（静态变量、静态语句块）
- 子类（静态变量、静态语句块）
- 父类（实例变量、普通语句块）
- 父类（构造函数）
- 子类（实例变量、普通语句块）
- 子类（构造函数）

### 正则表达式

https://juejin.im/post/5b5db5b8e51d4519155720d2

### 创建类的方式

1. 使用new``new Student()``
2. 使用反射机制，即Class类的newInstance方法，只能调用无参的

```java
Student student2 = (Student)Class.forName("Student类全限定名").newInstance();　
Student stu = Student.class.newInstance();
```

3. 使用反射机制，Constructor类的newInstance方法，可以调用有参数的

```java
Constructor<Student> constructor = Student.class.getConstructor(Integer.class);
Student stu3 = constructor.newInstance(123);
```

4. 使用clone方法创建心的对象们就必须先实现Cloneable接口并实现其定义的clone方法，这也是原型模式的应用
5. 使用反序列化机制创建对象；

### 反射中，Class.forName和ClassLoader区别 。

1. Class.forName

> Class.forName除了将类的.class文件加载到jvm中之外，还会对类进行解释，执行类中的static块;内部调用的是Class.forName(name,initialize,loader)带参数也可控制是否加载static块。并且只有调用了newInstance()方法采用调用构造函数，创建类的对象。

2. classLoader

> classLoader是类加载器，遵循双亲委派模型最终调用启动类加载器的类加载器，实现的功能是“通过一个类的全限定名来获取描述此类的二进制字节流”，获取到二进制流后放到JVM中。Class.forName()方法实际上也是调用的CLassLoader来实现的。并不自己进行加载static块；

### 栈溢出，堆溢出

1. 栈溢出

> java中栈是线程私有的，他的生命周期与线程相同，每个方法在执行的时候都会创建一个栈帧，用来存储局部变量表，操作数栈，动态链接，方法出口灯信息。局部变量表又包含基本数据类型，对象引用类型；主要造成原因是方法执行是创建的栈帧超过了栈的深度。那么最有可能的就是方法递归调用产生这种结果。
>
> 可以使用–Xss512k  设置线程栈大小为512k；

2. 堆溢出

> 堆中主要存储的是对象。如果不断的new对象则会导致堆中的空间溢出;或者堆内对象造成泄漏，以至于造成堆溢出；
>
> 可以使用:
>
> -Xmx4g   mx   堆最大内存
>
> -Xms4g   ms   堆初始内存
>
> -Xmn1200m  mn 年轻代的大小
>
> -XX：NewRatio=4   NewRatio 设置年老代（除去持久代）与年轻代（包括Eden和两个Survivor区的比值
>
> -XX:SurvivorRatio -XX:SurvivorRatio=8：设置年轻代中Eden区与Survivor区的大小比值。设置为8，则两个Survivor区与一个Eden区的比值为2:8
>
> -XX:PermSize=100m：初始化永久代大小为100MB。
> -XX:MaxPermSize=256m：设置持久代大小为256MB。

### try-catch-finally 中，如果 catch 中 return 了，finally 还会执行吗？

1、不管有木有出现异常，finally块中代码都会执行；
2、当try和catch中有return时，finally仍然会执行；
3、finally是在return后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来，管finally中的代码怎么样，返回的值都不会改变，任然是之前保存的值），所以函数返回值是在finally执行前确定的；
4、finally中最好不要包含return，否则程序会提前退出，返回值不是try或catch中保存的返回值。

# java容器

https://blog.csdn.net/xzp_12345/article/details/79251174

### hashmap

1. 阈值 为 大小 * 0.75
2. 大于阈值之后变进行扩容

### HashMap为什么不是线程安全的？

> hashmap 在添加Entry的时间，多线程操作可能会导致线程不安全；

### hashtable，hashmap，concurenthashmap的区别

### concurenthashmap jdk1.8改动

1. 利用CAS+synchronized来保证并发更新的安全，底层依然采用数组+链表+红黑树的存储结构。

2. basecount 记录元素数量，通过CAS更新

3. countercells 记录元素变化个数，cas操作basecount失败时使用。

4. 扩容的元素复制可并行进行。

# Java并发

### 线程状态

![](./img/thread.png)

start()

> 启动一个线程，线程此时为可运行状态，得到cpu之后便会运行；

run()

> run方法，是主题程序块，但是执行run方法知识简单的调用那个方法，start内部会调用run方法，但是同时也会设置thraedgroup

wait()

> wait方法是Object类的自带方法，调用执行之后，系统会释放资源，进入等待状态；当前线程必须拥有此对象的锁，所以必须出现在同步代码块中；避免 wait 和 notify 之间产生竞态条件。必须放在while循环中，是因为唤醒之后，可能不在执行判断，而是直接运行之后的diamagnetic；

notify()

> 唤醒调用wait的线程；能够唤醒一个正在等待这个对象锁线程，如果有多个线程都在等待这个对象的锁，则随机唤醒其中一个线程；

yield()

> 使当前线程从执行状态（运行状态）变为可执行态（就绪状态）。cpu会从众多的可执行态里选择，也就是说，当前也就是刚刚的那个线程还是有可能会被再次执行到的

sleep()

> 阻塞线程，不会释放资源，等待时间结束，自动继续运行

join()

> 调用其他线程的join方法，则等待其他线程运行完成之后，才继续进行自己的状态；

### 线程中断

> 一个线程执行完毕之后会自动结束，如果在运行过程中发生异常也会提前结束。

1. InterruptedException  interrupt() 

> 通过调用一个线程的 interrupt() 来中断该线程，如果该线程处于阻塞、限期等待或者无限期等待状态，那么就会抛出 InterruptedException，从而提前结束该线程。但是不能中断 I/O 阻塞和 synchronized 锁阻塞。

2. interrupted() 返回一个是否中断的boolen指

> 调用 interrupt() 方法会设置线程的中断标记，此时调用 interrupted() 方法会返回 true。因此可以在循环体中使用 interrupted() 方法来判断线程是否处于中断状态，从而提前结束线程。

3. 线程池中断操作

> 1. 使用shutdown()操作进行关闭线程池，shutdown内部关闭所有存活的线程
> 2. 只想中断 Executor 中的一个线程，可以通过使用 submit() 方法来提交一个线程，它会返回一个 Future<?> 对象，通过调用该对象的 cancel(true) 方法就可以中断线程。

### 守护线程

> 守护线程是程序运行时在后台提供服务的线程，不属于程序中不可或缺的部分。
>
> 当所有非守护线程结束时，程序也就终止，同时会杀死所有守护线程。
>
> main() 属于非守护线程。
>
> 使用 setDaemon() 方法将一个线程设置为守护线程。

### 线程安全

线程安全

> 多个线程不管以何种方式访问某个类，并且在主调代码中不需要进行同步，都能表现正确的行为。

实现方式

1. 使用不可变的类型，例如使用final修饰的基本数据类型，String，枚举类型，Number类的部分子类BigInteger 和 BigDecimal 等大数据类型。
2. 使用synchronized和ReentrantLock实现互斥同步
3. 使用CAS，乐观锁同步，使用非阻塞同步
4. 栈封闭，ThreadLocal，可重入代码均不需要进行同步；不依赖存储在堆上的数据和公用的系统资源、用到的状态量都由参数中传入、不调用非可重入的方法等。

### 乐观锁机制

> 先进行操作，如果没有其它线程争用共享数据，那操作就成功了，否则采取补偿措施（不断地重试，直到成功为止）；主要使用CAS实现

CAS

> CAS主要有三个操作值，分别是内存地址 V、旧的预期值 A 和新值 B。当执行操作时，只有当 V 的值等于 A，才将 V 的值更新为 B。是具有原子性的；由硬件机制保证；在java中主要通过unsafe类中的Java Native Interface本地调用c/c++语言实现的CAS；
>
> 例如compareAndSwapInt
>
> 缺点:
>
> 1. ABA问题；解决方法，为每个变量更新加上版本号
> 2. `CAS`操作失败，就需要循环进行`CAS`操作，自旋操作时间开销大；
> 3. 只能保证一个共享变量的原子操作；

### sleep 和 wait的区别

> sleep：
>
> - 让当前线程休眠指定时间
>
> - 不释放锁资源
>
> - 可通过调用interrupt()方法来唤醒休眠线程
>
> wait：
>
> - 让当前线程进入等待状态，当其他线程调用notify或者notifyAll方法时，当前线程进入就绪状态
>
> - 当前线程会释放已获取的锁资源，并进入等待队列

### 多线程三个特性
1. 原子性
    操作不可分割，可以使用synchronized关键字，和lock锁可以实现操作的原子性，另外AtomicInteger也具有原子性

2. 可见性
    一个线程对于共享变量的操作对其他线程是可见的，可以使用volatile 关键字定义变量；

3. 顺序性

   防止java代码优化重拍，volatile 关键字定义变量可以实现；

### 线程池

> 阿里为什么不让用自带的线程池创建
>
> 1）newFixedThreadPool和newSingleThreadExecutor:
>   主要问题是堆积的请求处理队列可能会耗费非常大的内存，甚至OOM。
> 2）newCachedThreadPool和newScheduledThreadPool:
>   主要问题是线程数最大数是Integer.MAX_VALUE，可能会创建数量非常多的线程，甚至OOM

##### 关键参数：

| 参数名称      | 作用                                               | 设置依据                                                     |
| ------------- | -------------------------------------------------- | ------------------------------------------------------------ |
| corePoolSzie  | 线程池核心线程数量                                 | 大多设置为核心数加一；IO密集型=2Ncpu；计算密集型=Ncpu        |
| mixPoolSize   | 线程池最大线程数量                                 |                                                              |
| taskQueue     | 储存任务队列（同步）                               | 表示如果任务数量超过核心池大小，多余的任务添加到阻塞队列中   |
| keepAliveTime | 非核心线程存活时间                                 |                                                              |
| threadFactory | 生成线程的工厂类                                   |                                                              |
| handler       | 任务队列满与线程数达到最大数量时间，执行的拒绝策略 | 直接丢弃（DiscardPolicy）丢弃队列中最老的任务(DiscardOldestPolicy)。抛异常(AbortPolicy)将任务分给调用线程来执行(CallerRunsPolicy)。 |
| unit          | 线程池维护线程所允许的空闲时间的单位               |                                                              |

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

### synchronized 和 lock

> 二者的区别:
>
> 1. synchronized 是Java内置关键字，Lock是Java类
>
> 2. synchronized 无法显式的判断是否获取锁的状态，Lock可以判断是否获取到锁
>
> 3. synchronized 会自动释放锁，Lock需要在finally中手工释放锁
>
> 4. synchronized 不同线程获取锁只有一个线程能获取成功，其他线程会一直阻塞直到获取锁，Lock有阻塞锁，也有非阻塞锁，阻塞锁还有尝试设置，功能更强
>
> 5. synchronized 可重入，不可中断，非公平，Lock锁可重入，可判断，有公平锁，非公平锁
>
> 6. Lock锁适合大量同步代码的同步问题，synchronized锁适合代码少量的同步问题

#### synchronized(jvm实现)

> Synchronized可以修饰普通方法、同步方法块、静态方法；(可重入)
>  普通方法锁是当前实例对象，静态方法锁是当前类的Class对象，同步方法块锁是Synchonized配置的对象；
>  用的锁是存在对象头里的,根据mark word的锁状态来判断锁，如果锁只被同一个线程持有使用的是偏向锁，不同线程互相交替持有锁使用轻量级锁，多线程竞争使用重量级锁。锁会按偏向锁->轻量级锁->重量级锁 升级，称为锁膨胀

##### 可重入的实现

> 为synchronized使用的是锁对象，当某个线程第一次持有锁后，会修改锁对象的mark word锁状态为偏向锁，偏向锁锁会在当前线程的栈帧中建立一个锁记录空间，mark word会将指针指向栈中的锁记录。当线程再次获取锁对象的时候，会检查mark word 中的指针是否指向当前线程的栈帧，如果是就直接获取锁，如果不是就需要竞争;偏向锁可能就会进化为重量级锁

#### lock

> lock是一个接口类，实现主要有ReentrantLock可重入锁，ReadLock读锁，WriteLock写锁等；主要创建方式是通过多态的方式进行的创建；

##### ReentrantLock(jdk实现)

> 底层是通过AQS实现的，每次获取锁，均会通过CAS使得state加一，而2其他线程想要获取锁的时候，只能等待这个state为0才能通过AQS队列获取这个锁；最常用的锁
>
> 能够使用重入锁的的必须保证

##### ReentrantLock中的lock和unlock之间的同步如何进行线程间的通信

> 实际就是使用synchronized关键字同步中用到的wait和notify,notifyAll方法的类似功能。ReentrantLock采用的是Condition接口中的await等待，signal方法进行唤醒。Condition这个接口的类型是通过ReentrantLock的实例newCondition()进行创建的类型;

### AQS思想

https://juejin.im/entry/5ae02a7c6fb9a07ac76e7b70

> AQS使用一个int类型的成员变量state来表示同步状态，当state>0时表示已经获取了锁，当state = 0时表示释放了锁。它提供了三个方法（getState()、setState(int newState)、compareAndSetState(int expect,int update)）来对同步状态state进行操作，当然AQS可以确保对state的操作是安全的。
>
> AQS通过内置的FIFO同步队列来完成资源获取线程的排队工作，如果当前线程获取同步状态失败（锁）时，AQS则会将当前线程以及等待状态等信息构造成一个节点（Node）并将其加入同步队列，同时会阻塞当前线程，当同步状态释放时，则会把节点中的线程唤醒，使其再次尝试获取同步状态。

###  CountDownLatch、CyclicBarrier、Semaphore

CountDownLatch

> 主要是使用await和countdown方法；

```java
public class AQSDemo {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch countDownLatch = new CountDownLatch(10);
        ExecutorService service = Executors.newCachedThreadPool();
        for (int i = 0; i < 10; i++) {
            service.execute(new Thread(new Runnable() {
                @Override
                public void run() {
                    System.out.print("run..");
                    countDownLatch.countDown();
                }
            }));
        }
        countDownLatch.await();
        System.out.println("end");
        service.shutdown();
    }
}
```

CyclicBarrier

> 一般只使用await方法，所有线程执行await方法之后，便运行后续程序；

```java
final int totalThread = 10;
CyclicBarrier cyclicBarrier = new CyclicBarrier(totalThread);
ExecutorService executorService = Executors.newCachedThreadPool();
for (int i = 0; i < totalThread; i++) {
    executorService.execute(() -> {
        System.out.print("before..");
        try {
            cyclicBarrier.await();
        } catch (InterruptedException | BrokenBarrierException e) {
            e.printStackTrace();
        }
        System.out.print("after..");
    });
}
executorService.shutdown();
}
```

Semaphore

>类似操作系统的信号量，主要依靠acquire和release两个方法

```java
try {
    mutex.acquire();
    System.out.print("A");
} catch (Exception e) {
    e.printStackTrace();
}finally {
    mutex.release();
    B.release();
}
```



### ⭐公平锁和非公平锁的实现

https://blog.csdn.net/qyp199312/article/details/70598480

### ThreadLocal原理，如何使用？

### wait,notify的过程

> 一定要获取锁，所以必须出现在同步代码块中

#### lock中的使用

> wait()，notify()及notifyAll()只能在synchronized语句中使用，但是如果使用的是ReenTrantLock实现同步，该如何达到这三个方法的效果呢？解决方法是使用ReenTrantLock.newCondition()获取一个Condition类对象，然后Condition的await()，signal()以及signalAll()分别对应上面的三个方法

# java虚拟机

### java运行时间内存划分

![](./img/javavm.png)

### 类加载过程

1. 加载

> 1. 通过类的完全限定名称获取定义该类的二进制字节流。
> 2. 将该字节流表示的静态存储结构转换为方法区的运行时存储结构。defineClass
> 3. 在内存中生成一个代表该类的 Class 对象，作为方法区中该类各种数据的访问入口。

2. 验证

> 确保 Class 文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。

3. 准备

> 类变量是被 static 修饰的变量，准备阶段为类变量分配内存并设置初始值，使用的是方法区的内存。一般情况下初始值为0值，但是如果使用了final修饰，则初值为代码中的值；

4. 解析

> 将常量池的符号引用替换为直接引用的过程。

5. 初始化

> 初始化阶段才真正开始执行类中定义的 Java 程序代码,但是仍然是类的变量，或者是方法，而不是对象的；在准备阶段，类变量已经赋过一次系统要求的初始值，而在初始化阶段，根据程序员通过程序制定的主观计划去初始化类变量和其它资源

#### 双亲委派模型

> 过程：一个类加载器首先将类加载请求转发到父类加载器，只有当父类加载器无法完成时才尝试自己加载。
>
> 好处: 使得 Java 类随着它的类加载器一起具有一种带有优先级的层次关系，从而使得基础类得到统一。

#### 破坏双亲委派模型

> 例如:DriverManager（也由jdk提供）要加载各个实现了Driver接口的实现类，然后进行管理，但是DriverManager由启动类加载器加载，只能记载JAVA_HOME的lib下文件，而其实现是由服务商提供的，由系统类加载器加载，这个时候就需要启动类加载器来委托子类来加载Driver实现，从而破坏了双亲委派，这里仅仅是举了破坏双亲委派的其中一个情况。

#### 类加载器分类

1. 启动类加载器 Bootstrap ClassLoader
2. 扩展类加载器
3. 应用程序类加载器
4. 自定义加载器

> 它首先根据类的全名在文件系统上查找类的字节代码文件（.class 文件），然后读取该文件内容，最后通过 defineClass() 方法来把这些字节代码转换成 java.lang.Class 类的实例。
>
> java.lang.ClassLoader 的 loadClass() 实现了双亲委派模型的逻辑，自定义类加载器一般不去重写它，但是需要重写 findClass() 方法。

### java内存管理

#### Mirror GC 和 FULL GC

1. mirror gc  新生代空间不足就会发生一次，速度快，回收新生代
2. full gc：回收老年代和新生代，老年代对象其存活时间长，因此 Full GC 很少执行，执行速度会比 Minor GC 慢很多。

#### 堆内存分配策略

1. 对象优先进入新生代
2. 大对象直接进入老年代
3. 长期存活的对象进入老年代，经过Mirror GC还存活的对象年龄加一，动态对象年龄判定
4. 空间分配担保

#### FULL GC的触发条件

1. System.gc()调用
2. 空间担保失败
3. 老年代空间不足

### GC

#### 判断一个对象是否可以回收

1. 引用计数法
2. 可达性分析算法

> 可以作为gc root
>
> 1. 虚拟机栈中局部变量表中引用的对象
> 2. 本地方法栈中 JNI 中引用的对象
> 3. 方法区中类静态属性引用的对象
> 4. 方法区中的常量引用的对象

1. 方法区回收
2. finalize()

#### 类卸载条件

1. 该类的所有实例均已经被回收
2. 该类的类加载器已经被回收
3. 该类对应的 Class 对象没有在任何地方被引用，也就无法在任何地方通过反射访问该类方法。

#### 垃圾收集器

1. Serial收集器：单线程收集器
2. ParNew收集器：serial的多线程版本
3. Parallel Scavenge 收集器： ParNew 一样是多线程收集器。
4. Serial Old收集器：serial的老年代版本
5. Parallel Old收集器：Parallel Scavenge老年代版本
6. CMS收集器

过程:

- 初始标记：仅仅只是标记一下 GC Roots 能直接关联到的对象，速度很快，需要停顿。
- 并发标记：进行 GC Roots Tracing 的过程，它在整个回收过程中耗时最长，不需要停顿。
- 重新标记：为了修正并发标记期间因用户程序继续运作而导致标记产生变动的那一部分对象的标记记录，需要停顿。
- 并发清除：不需要停顿。

缺点:

- 吞吐量低：低停顿时间是以牺牲吞吐量为代价的，导致 CPU 利用率不够高。
- 无法处理浮动垃圾，可能出现 Concurrent Mode Failure。浮动垃圾是指并发清除阶段由于用户线程继续运行而产生的垃圾，这部分垃圾只能到下一次 GC 时才能进行回收。由于浮动垃圾的存在，因此需要预留出一部分内存，意味着 CMS 收集不能像其它收集器那样等待老年代快满的时候再回收。如果预留的内存不够存放浮动垃圾，就会出现 Concurrent Mode Failure，这时虚拟机将临时启用 Serial Old 来替代 CMS。
- 标记 - 清除算法导致的空间碎片，往往出现老年代空间剩余，但无法找到足够大连续空间来分配当前对象，不得不提前触发一次 Full GC。

7. G1收集器

过程

- 初始标记
- 并发标记
- 最终标记：为了修正在并发标记期间因用户程序继续运作而导致标记产生变动的那一部分标记记录，虚拟机将这段时间对象变化记录在线程的 Remembered Set Logs 里面，最终标记阶段需要把 Remembered Set Logs 的数据合并到 Remembered Set 中。这阶段需要停顿线程，但是可并行执行。
- 筛选回收：首先对各个 Region 中的回收价值和成本进行排序，根据用户所期望的 GC 停顿时间来制定回收计划。此阶段其实也可以做到与用户程序一起并发执行，但是因为只回收一部分 Region，时间是用户可控制的，而且停顿用户线程将大幅度提高收集效率。

优点:

- 空间整合：整体来看是基于“标记 - 整理”算法实现的收集器，从局部（两个 Region 之间）上来看是基于“复制”算法实现的，这意味着运行期间不会产生内存空间碎片。
- 可预测的停顿：能让使用者明确指定在一个长度为 M 毫秒的时间片段内，消耗在 GC 上的时间不得超过 N 毫秒。

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

### 创建对象的过程(new)

![](./img/new.png)

### jvm常用命令

#### jvm调优和监控

在Java应用和服务出现莫名的卡顿、CPU飙升等问题时总是要分析一下对应进程的JVM状态以定位问题和解决问题并作出相应的优化，在这过程中Java自带的一些状态监控命令和图形化工具就非常方便了。本文总结了最常用的命令行工具及其常用参数解释，图形化监控工具的用法

#### jps

> 显示当前运行的java进程以及相关参数

```shell
jsp -l pid
-q 只显示pid，不显示class名称,jar文件名和传递给main 方法的参数。
-l 输出应用程序main class的完整package名 或者 应用程序的jar文件完整路径名。
-m 输出传递给main方法的参数
-v 输出传递给JVM的参数
```

#### jstack

> 用于生成java虚拟机当前时刻的线程快照。

###### 分析CPU利用率100%问题

1. top 查看占CPU最多的进程
2. top -Hp pid 查询进程下所有线程的运行情况（shift+p 按cpu排序，shift+m 按内存排序）
3. 用printf ‘%x’ pid 转换为16进制（加入查到的是a）
4. jstact查看线程快照，jstack 30316 | grep -A 20 a

#### jmap

> 用于打印指定Java进程(或核心文件、远程调试服务器)的共享对象内存映射或堆内存细节

1. 看java堆（heap）使用情况：jmap -heap 31846

2. 查看java堆（heap）中的对象数量及大小：jmap -histo 31846

3. 将内存使用的详细情况输出到文件： jmap -dump:format=b,file=heapDump pid然后使用jhat -port 5000 heapDump在浏览器中访问：[http://localhost:5000/](https://link.jianshu.com?t=http://localhost:5000/)查看详细信息

#### jinfo

> jinfo可以输出java进程、core文件或远程debug服务器的配置信息。可以使用jps -v替换

```shell
jinfo [option] <pid>
options参数解释：
-flag <name> 打印指定名称的参数
-flag [+|-]<name> 打开或关闭参数
-flag <name>=<value> 设置参数
-flags 打印所有参数
-sysprops 打印系统配置
<no option> 打印上面两个选项
```

#### jstat

> 是用于监控虚拟机各种运行状态信息的命令行工具。他可以显示本地或远程虚拟机进程中的类装载、内存、垃圾收集、JIT编译等运行数据。

jstat -<option> [-t] [-h<lines>] <vmid> [<interval> [<count>]]
 参数解释：

Option — 选项，我们一般使用 -gcutil 查看gc情况

vmid — VM的进程号，即当前运行的java进程号

interval– 间隔时间，单位为秒或者毫秒

count — 打印次数，如果缺省则打印无数次

例子：jstat -gc 5828 250 5

#### javap

> 可以对代码反编译，也可以查看java编译器生成的字节码。

#### jconsole, jvisualvm

> 图形化工具

### JVM内存为什么要分成新生代，老年代，持久代。新生代中为什么要分为Eden和Survivor。

1. survivor的作用

> 由于经过一次GC之后，新生代如果没有survivor区，新生代幸存对象便会直接进入老年代；但是这样这样可能下一次gc这个对象就不会使用，这样就会造成老年代空间浪费；所以使用survivor区，只有将两个survivor区互相多次复制；才进入老年代；设置 Survivor 空间的目的是让那些中等寿命的对象尽量在 Minor GC 时被干掉，最终在总体上减少虚拟机的垃圾收集过程对用户程序的影响。
>
> 需要两个survivor的原因:gc 时把存活的对象从一块空间（From space）复制到另外一块空间（To space），再把原先的那块内存（From space）清理干净，最后调换 From space 和 To space 的逻辑角色

### 一次完整的jvm垃圾收集机制

> 对象诞生即新生代->eden，在进行mirror gc过程中，如果依旧存活，移动到from，变成Survivor，进行标记。当一个对象存活默认超过15次都没有被回收掉，就会进入老年代。如果在使用mirror gc过程中，空间担保失败，就会发生fullGC；如果最后进入老年代，然而老年代空间不足，也会发生FULL GC；

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

https://segmentfault.com/a/1190000011291179

##### 静态代理

> 这种代理方式需要代理对象和目标对象实现一样的接口。
>
> 优点：可以在不修改目标对象的前提下扩展目标对象的功能。
>
> 缺点：
>
> 1. 冗余。由于代理对象要实现与目标对象一致的接口，会产生过多的代理类。
> 2. 不易维护。一旦接口增加方法，目标对象与代理对象都要进行修改。

##### 动态代理

1. java自带的代理，主要用于代理一个实现接口的类时，使用java自带的代理(接口代理)

> 主要涉及java.lang.reflect中的两个类，Proxy 和 InvocationHandler 
> InvocationHandler是一个接口，通过实现该接口定义横切逻辑，并通过反射机制调用目标类的代码，动态将横切逻辑和业务逻辑编辑在一起。只能为实现接口的类创建代理;
>
> 除了public之外的其他所有方法都不能进行代理，缺省也不行，**public static**也不行

1. cglib代理，除去上述方式其他的使用这个进行代理(类代理)

> 是一个强大的高性能，高质量的代码生成类库，可以在运行期扩展Java类与实现Java接口，Cglib封装了asm,可以在运行期动态生成新的class。可以是普通类，也可以是实现接口的类;动态生成一个子类对代理目标类进行扩充；
>
> 由于其通过生成目标类子类的方式来增强，因此**不能被子类继承的方法都不能被增强**，private、static、final 方法

两种代理的区别

#### IOC(控制反转) 

甲乙双方不相互依赖，交易活动的进行不依赖于甲乙任何一方，整个活动的进行由第三方负责管理。资源集中管理，实现资源的可配置和易管理。第二，降低了使用资源双方的依赖程度，也就是我们说的耦合度。

##### 几种scope的区别

1. singlon 单例，系统中只有一个实例
2. protype 原型，每次通过容器的getBean方法获取prototype定义的Bean时，都将产生一个新的Bean实例
3. request ，对于每次HTTP请求，使用request定义的Bean都将产生一个新实例，即每次HTTP请求将会产生不同的Bean实例。只有在Web应用中使用Spring时，该作用域才有效
4. session ，对于每次HTTP Session，使用session定义的Bean都将产生一个新实例。同样只有在Web应用中使用Spring时，该作用域才有效
5. globalsession：每个全局的HTTP Session，使用session定义的Bean都将产生一个新实例。典型情况下，仅在使用portlet context的时候有效。同样只有在Web应用中使用Spring时，该作用域才有效

#### DI(依赖注入)

则是，甲方开放接口，在它需要的时候，能够讲乙方传递进来(注入)

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

#### spring 中的 bean 是线程安全的吗？

> 原型类型的bean因为不存在共享，所以是安全的，单例bean如果是无状态的也是线程安全的；

**Spring根本就没有对bean的多线程安全问题做出任何保证与措施**。对于每个bean的线程安全问题，根本原因是每个bean自身的设计。**不要在bean中声明任何有状态的实例变量或类变量，如果必须如此，那么就使用ThreadLocal把变量变为线程私有的，如果bean的实例变量或类变量需要在多个线程之间共享，那么就只能使用synchronized、lock、CAS等这些实现线程同步的方法了。**

#### spring 自动装配 bean 有哪些方式？

1. no   默认的方式是不进行自动装配，通过手工设置ref 属性来进行装配bean
2. byName   通过参数名 自动装配，如果一个bean的name 和另外一个bean的 property 相同，就自动装配。
3. byType   通过参数的数据类型自动自动装配，如果一个bean的数据类型和另外一个bean的property属性的数据类型兼容，就自动装配
4. construct   构造方法中的参数通过byType的形式，自动装配。
5. autodetect   如果有默认的构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配。

#### spring事务

juejin.im/post/5b00c52ef265da0b95276091

#### spring线程安全

https://blog.csdn.net/m0_37222746/article/details/56486694

### mybaits

#### MyBatis中#{}和${}的区别

1. 使用${}方式传入的参数，mybatis不会对它进行特殊处理，而使用#{}传进来的参数，mybatis默认会将其当成字符串；
2. \#和$在预编译处理中是不一样的。#类似jdbc中的PreparedStatement，对于传入的参数，在预处理阶段会使用**?代替**，待真正查询的时候即在数据库管理系统中（DBMS）才会代入参数。而${}则是简单的**替换**



#### mybaits缓存

##### 一级缓存

> 一级缓存是SqlSession级别的缓存。在操作数据库时需要构造 sqlSession对象，在对象中有一个(内存区域)数据结构（HashMap）用于存储缓存数据。不同的sqlSession之间的缓存数据区域（HashMap）是互相不影响的。一级缓存的作用域是同一个SqlSession，在同一个sqlSession中两次执行相同的sql语句，第一次执行完毕会将数据库中查询的数据写到缓存（内存），第二次会从缓存中获取数据将不再从数据库查询，从而提高查询效率。当一个sqlSession结束后该sqlSession中的一级缓存也就不存在了。Mybatis默认开启一级缓存。

注意点:

1. 如果sqlsession执行过插入删除更新操作，那么会清空一级缓存；
2. 执行两次service调用查询相同的用户信息，不走一级缓存，因为Service方法结束，sqlSession就关闭，一级缓存就清空。

##### 二级缓存

> 二级缓存是mapper级别的缓存，多个SqlSession去操作同一个Mapper的sql语句，多个SqlSession去操作数据库得到数据会存在二级缓存区域，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的。其作用域是mapper的同一个namespace，不同的sqlSession两次执行相同namespace下的sql语句且向sql中传递参数也相同即最终执行相同的sql语句，第一次执行完毕会将数据库中查询的数据写到缓存（内存），第二次会从缓存中获取数据将不再从数据库查询，从而提高查询效率



### springboot

#### 自动配置

##### 三个重要注解

- `@SpringBootConfiguration`：我们点进去以后可以发现底层是**Configuration**注解，说白了就是支持**JavaConfig**的方式来进行配置(**使用Configuration配置类等同于XML文件**)。
- `@EnableAutoConfiguration`：开启**自动配置**功能(后文详解)
- `@ComponentScan`：这个注解，学过Spring的同学应该对它不会陌生，就是**扫描**注解，默认是扫描**当前类下**的package。将`@Controller/@Service/@Component/@Repository`等注解加载到IOC容器中。

其中`@EnableAutoConfiguration`是关键(启用自动配置)，内部实际上就去加载`META-INF/spring.factories`文件的信息，然后筛选出以`EnableAutoConfiguration`为key的数据，加载到IOC容器中，实现自动配置功能！

#### 注解的原理和实现

https://juejin.im/post/5b45bd715188251b3a1db54f

#### .spring boot 有哪些方式可以实现热部署？

1. devtools

```xml
<!-- 热部署依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
    <!-- optional=true,依赖不会传递，该项目依赖devtools；之后依赖myboot项目的项目如果想要使用devtools，需要重新引入 -->
</dependency>

<!-- 添加下面的fork属性为true-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <!--fork :  如果没有该项配置，devtools不会起作用，即应用不会restart -->
                <fork>true</fork>
            </configuration>
        </plugin>
    </plugins>
</build>
```

2. springloaded

### tomcat类加载

https://www.cnblogs.com/xing901022/p/4574961.html

> 会优先加载java类编译的class文件
>
> 1. 使用bootstrap引导类加载器加载
> 2. 使用system系统类加载器加载
> 3.  使用应用类加载器在WEB-INF/classes中加载
> 4. 使用应用类加载器在WEB-INF/lib中加载
> 5. 用common类加载器在CATALINA_HOME/lib中加载







