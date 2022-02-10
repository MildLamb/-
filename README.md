## 什么是面向对象?
- 对比面向过程，是两种不同的处理问题的角度  
- 面向过程更注重事情的每一个步骤及顺序，面向对象更注重事情有哪些参与者(对象)、及各自需要做什么

## JDK,JRE,JVM的联系和区别?
- JDK：Java Develpment Kit(Java开发工具包)
- JRE：Java Runtime Environment(Java运行时环境)
- JVM：Java Virtual(Java虚拟机)

![image](https://user-images.githubusercontent.com/92672384/152710278-d4c118f2-db7b-4119-8571-daa61487e946.png)

## ==和equals的区别?
- ==对比的是栈中的值，基本数据类型是变量值，引用类型是堆中内存对象的地址

## final的作用?
最终的  
- 修饰类：表示该类为最终类，这个类不可以再被继承
- 修饰方法：表示该方法为最终方法，表示方法不能被子类重写，但是可以重载
- 修饰变量：表示变量一旦被赋值就不可以再改变它的值
(1) 修饰成员变量
- 如果final修饰的是(static)类变量，只能在静态初始化块中指定初始值或者声明该类变量时指定初始值。
- 如果final修饰的是成员变量，可以在非静态初始化块、声明该变量或者构造器中执行初始值。

![image](https://user-images.githubusercontent.com/92672384/152711231-d10d6a82-9f6f-48a3-8da3-3323cdf1e90e.png)
(2) 修饰局部变量
- 系统不会为局部变量进行初始化，局部变量必须由程序员显示初始化。因此使用final修饰局部变量时，即可以在定义时指定默认值(后面的代码不能对变量再赋值)，也可以不指定默认值，而在后面的代码中对final变量赋初值(仅一次)

![image](https://user-images.githubusercontent.com/92672384/152711683-e916abd4-9429-470b-b22d-396076a32c61.png)

(3) 修饰基本类型数据和引用类型数据
- 如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改
- 如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。但是**引用的值是可变的**。

## 为什么局部内部类和匿名内部类只能访问局部final变量(JDK8默认会加上final，所以不会报错)?
- 下面代码编译后会生成两个class文件，Test.class和Test1.class
```java
public class Test {
    public static void main(String[] args) {
        new OutClass().outPrint(23);
    }

    // 局部final变量 a,b
    public void test(int b){
        int a = 10;
        new Thread(){
            @Override
            public void run() {
                System.out.println(a);
                System.out.println(b);
            }
        }.start();
    }

}


class OutClass {
    private int age = 12;
    public void outPrint(int x) {
        class InClass {
            public void InPrint() {
                System.out.println(x);
                System.out.println(age+1);
            }
        }
        new InClass().InPrint();
    }
}
```
- 首先需要知道的一点是:内部类和外部类是处于同一个级别的，内部类不会因为定义在方法中就会随着方法的执行完毕就被销毁。
- 这里就会产生问题:当外部类的方法结束时，局部变量就会被销毁了，但是内部类对象可能还存在(只有没有人再引用它时，才会死亡)。这里就出现了一个矛盾:内部类对象访问了一个不存在的变量。为了解决这个问题，就将局部变量复制了一份作为内部类的成员变量，这样当局部变量死亡后，内部类仍可以访问它，实际访问的是局部变量的"copy"。这样就好像延长了局部变量的生命周期

## String,StringBuffer,SreingBuilder区别以及使用场景?
- String是final修饰的，不可变，每次操作都会产生新的String对象
- StringBuffer和StringBuilder都是可以在原对象上操作
- StringBuffer是线程安全的，StringBuilder是线程不安全的
- StringBuffer的方法都是有synchronized修饰的
- 性能：StringBuilder > StringBuffer > String

场景：需要经常改变字符串内容的时候使用后两个  
优先使用StringBuilder，多线程共享变量时使用StringBuffer  

## 重载和重写的区别?
- 重载:发生在同一个类中，方法名必须相同，参数类型不同、个数不同、顺序不同,方法返回值和访问修饰符可以不同(意思就是任然报错)，发生在编译时。  
- 重写:发生在父子类中，方法名、参数列表必须相同，返回值范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类;如果父类方法访问修饰符为private则子类就不能重写该方法。

## 接口和抽象类的区别?
- 抽象类可以存在普通成员函数，而接口中只能存在public abstract方法。
- 抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是public static final类型的。
- 抽象类只能继承一个，接口可以实现多个。

## List和Set的区别?
- List:有序，按对象进入的顺序保存对象，可重复，允许多个Null元素对象，可以使用lterator取出所有元素，在逐一遍历，还可以使用get(int index)获取指定下表的元素
- Set:无序，不可重复，最多允许有一个Null元素对象，取元素时只能用lterator接口取得所有元素，在逐一遍历各个元素

## hashCode和equals
### hashCode介绍
hashCode()的作用是获取哈希码，也称为散列码;它实际上是返回一个int整数。这个哈希码的作用是确定该对象在哈希表中的索引位置。hashCode()定义在JDK的Object.java中，Java中的任何类都包含有hashCode()函数。散列表存储的是键值对(key-value)，它的特点是:能根据"键"快速的检索出对应的“值"。这其中就利用到了散列码!(可以快速找到所需要的对象)

- 如果两个对象相等，则hashcode—定也是相同的
- 两个对象相等,对两个对象分别调用equals方法都返回true
- 两个对象有相同的hashcode值，它们也不一定是相等的因此
- equals方法被覆盖过，则hashCode方法也必须被覆盖
- hashCode()的默认行为是对堆上的对象产生独特值。如果没有重写hashCode()，则该class的两个对象无论如何都不会相等（即使这两个对象指向相同的数据)

## ArrayList和LinkedList区别
- ArrayList:基于动态数组，连续内存存储，适合下标访问（随机访问)，扩容机制:因为数组长度固定，超出长度存数据时需要新建数组，然后将老数组的数据拷贝到新数组，如果不是尾部插入数据还会涉及到元素的移动(往后复制一份，插入新元素)，使用尾插法并指定初始容量可以极大提升性能、甚至超过linkedList(需要创建大量的node对象)
- LinkedList:基于链表，可以存储在分散的内存中，适合做数据插入及删除操作，不适合查询:需要逐一遍历遍历LinkedList必须使用iterator不能使用for循环，因为每次for循环体内通过get(i取得某一元素时都需要对list重新进行遍历，性能消耗极大。另外不要试图使用indexOf等返回元素索引，并利用其进行遍历，使用indexlof对list进行了遍历，当结果为空时会遍历整个列表。

## HashMap和Hashtable的区别
(1) HashMap方法没有synchronized修饰，线程非安全，HashTable线程安全;  
(2) HashMap允许key和value为null，而HashTable不允许  

jdk8开始链表高度到8、数组长度超过64，链表转变为红黑树，元素以内部类Node节点存在  
- 计算key的hash值，二次hash然后对数组长度取模，对应到数组下标
- 如果没有产生hash冲突(下标位置没有元素)，则直接创建Node存入数组
- 如果产生hash冲突，先进行equal比较，相同则取代该元素，不同，则判断链表高度插入链表，链表高度达到8，并且数组长度到64则转变为红黑树，长度低于6则将红黑树转回链表
- key为null，存在下标0的位置

容量默认是16，加载因子默认是0.75，阈值=容量*加载因子，默认是12，当元素数量超过阈值时便会触发扩容，扩容为原来的2倍

## ConcurrentHashMap原理，jdk7和jdk8的区别
jdk7：  
- 数据结构: ReentrantLock+Segment+HashEntry，一个Segment中包含一个HashEntry数组，每个HashEntry又是一个链表结构Ⅰ  
- 元素查询: 二次hash，第一次Hash定位到Segment，第二次Hash定位到元素所在的链表的头部   
- 锁:Segment分段锁Segment继承了ReentrantLock，锁定操作的Segment，其他的Segment不受影响，并发度为segment个数，可以通过构造函数指定，数组扩容不会影响其他的segment  
- get方法无需加锁,volatile保证

jdk8：  
- 数据结构: synchronized+CAS+Node+红黑树，Node的val和next都用volatile修饰，保证可见性查找，替换，赋值操作都使用CAS
- 锁:锁链表的head节点，不影响具他兀系的读与，认性，扩容
- 读操作无锁:Node的val和next使用volatile修饰，读写线程对该变量互相可见数组用volatile修饰，保证扩容时被读线程感知

## 如何实现一个IOC容器?
1. 配置文件配置包扫描路径
2. 递归包扫描获取.class文件
3. 反射、确定需要交给Ioc管理的类
4. 对需要注入的类进行依赖注入

- 配置文件中指定需要扫描的包路径
- 从配置文件中获取需要扫描的包路径，获取到当前路径下的文件信息及文件夹信息，我们将当前路径下所有以.class结尾的文件添加到一个Set集合中进行存储
- 遍历这个set集合，获取在类上有指定注解的类，并将其交给IOC容器，定义一个安全的Map用来存储这些对象
- 遍历这个IOC容器，获取到每一个类的实例，判断里面是有有依赖其他的类的实例，然后进行递归注入

## 什么是字节码?使用字节码有什么好处?
- Java中引入了虚拟机的概念，即在机器和编译程序力间加入了一层抽象的虚拟的机器。这台虚拟的机器在任何平台上都提供给编译程序一个的共同的接口。  
- 编译程序只需要面向虚拟机，生成虚拟机能够理解的代码，然后由解释器来将虚拟机代码转换为特定系统的机器码执行。在Java中，这种供虚拟机理解的代码叫做字节码(即扩展名为.class的文件)，它不面向任何特定的处理器，只面向虚拟机。  
好处：  
Java语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。所以ava程序运行时比较高效，而且，由于字节码并不专对一种特定的机器，因此，Java程序无须重新编译便可在多种不同的计算机上运行。

## Java的类加载器
- JDK自带有三个类加载器: bootstrap ClassLoader、ExtClassLoader、AppClassLoader。
- BootStrapClassLoader是ExtClassLoader的父类加载器，默认负责加载%JAVA_HOME%lib下的jar包和class文件。
- ExtClassLoader是AppClassLoader的父类加载器，负责加载%JAVA_HOME%/lib/ext文件夹下的jar包和class类。
- AppClassLoader是自定义类加载器的父类，负责加载classpath下的类文件。
- 继承ClassLoader实现自定义类加载器

## 双亲委派机制
![image](https://user-images.githubusercontent.com/92672384/153102744-c4c0ae09-7fb2-4405-b0ee-3fff2a579c91.png)

双亲委派的好处：  
- 主要是为了安全性，避免用户自己编写的类动态替换Java的一些核心类，比如String。
- 同时也避免了类的重复加载，因为JⅣM中区分不同类，不仅仅是根据类名，相同的class文件被不同的ClassLoader加载就是不同的两个类

## Java的异常体系
- Java中的所有异常都来自顶级父类Throwable。
- Throwable下有两个子类Exception和Error（如OOM，OutOfMemory）。
- Error是程序无法处理的错误，一旦出现这个错误，则程序将被迫停止运行。
- Exception不会导致程序停止，又分为两个部分RunTimeException运行时异常和CheckedException检查异常。
- RunTimeException常常发生在程序运行过程中，会导致程序当前线程执行失败。CheckedException常常发生在程序编译过程中，会导致程序编译不通过。

1、常见编译时异常：  

- IOException                    输入输出流异常 
- FileNotFoundException          文件找不到的异常  
- ClassNotFoundException         类找不到异常  
- DataFormatException            数据格式化异常  
- NoSuchFieldException           没有匹配的属性异常  
- NoSuchMethodException          没有匹配的方法异常  
- SQLException                   数据库操作异常  
- TimeoutException               执行超时异常  

2、运行时异常（RuntimeException）：  

- ArrayIndexOutofBoundsException     数组越界异常 
- ClassCastException                 类型转换异常 
- NullPointerException               空指针异常   
- IllegalAccessException             非法的参数异常 
- InputMismatchException             输入不匹配异常
- ArithmeticException                算术异常

## GC如何判断对象可以回收
- 引用计数法:每个对象有一个引用计数属性，新增一个引用时计数加1，引用释放时计数减1，计数为0时可以回收
- 可达性分析法:从GCRoots开始向下搜索，搜索所走过的路径称为引用链。当一个对象到GCRoots没有任何引用链相连时，则证明此对象是不可用的，那么虚拟机就判断是可回收对象。

GC Root：  
- 虚拟机栈(栈帧中的本地变量表)中引用的对象
- 方法区中类静态属性引用的对象
- 方法区中常量引用的对象
- 本地方法栈中JNI(即一般说的Native方法)引用的对象

## 线程的生命周期，线程有哪些状态?
1. 线程通常有五种状态，创建，就绪，运行、阻塞和死亡状态。
2. 阻塞的情况又分为三种:  
(1)、等待阻塞:运行的线程执行wait方法，该线程会释放占用的所有资源，JVM会把该线程放入“等待池"中。进入这个状态后，是不能自动唤醒的，必须依靠其他线程调用notify或notifyAll方法才能被唤醒,wait是object类的方法  
(2)、同步阻塞:运行的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则VM会把该线程放入"锁池"中。  
(3)、其他阻塞:运行的线程执行sleep或join方法，或者发出了IO请求时，JVM会把该线程置为阻塞状态。当sleep状态超时、 join等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。sleep是Thread类的方法  

1. 新建状态(New) :新创建了一个线程对象。
2. 就绪状态(Runnable):线程对象创建后，其他线程调用了该对象的start方法。该状态的线程位于可运行线程池中，变得可运行，等待获取CPU的使用权。
3. 运行状态(Running)∶就绪状态的线程获取了CPU，执行程序代码。
4. 阻塞状态(Blocked):阻塞状态是线程因为某种原因放弃CPU使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。
5. 死亡状态(Dead):线程执行完了或者因异常退出了run方法，该线程结束生命周期。

## sleep，wait，join，yeild的区别?
1. 锁池
所有需要竞争同步锁的线程都会放在锁池当中，比如当前对象的锁已经被其中一个线程得到，则其他线程需要在这个锁池进行等待，当前面的线程释放同步锁后锁池中的线程去竞争同步锁，当某个线程得到后会进入就绪队列进行等待cpu资源分配。  
2. 等待池
当我们调用wait()方法后，线程会放到等待池当中，等待池的线程是不会去竞争同步锁。只有调用了notify ()或notifyAll()后等待池的线程才会开始去竞争锁，notify ()是随机从等待池选出一个线程放到锁池，而notifyAlI()是将等待池的所有线程放到锁池当中

1. sleep是Thread类的静态本地方法，wait则是Object类的本地方法。
2. sleep方法不会释放lock，但是wait会释放，而且会加入到等待队列中。
3. sleep方法不依赖于同步器synchronized，但是wait需要依赖synchronized关键字。
4. sleep不需要被唤醒（休眠之后退出阻塞)，但是wait需要（不指定时间需要被别人中断)。
5. sleep一般用于当前线程休眠，或者轮循暂停操作，wait则多用于多线程之间的通信。
6. sleep会让出CPU执行时间且强制上下文切换，而wait则不一定，wait后可能还是有机会重新竞争到锁继续执行的。

- yield ()执行后线程直接进入就绪状态，马上释放了cpu的执行权，但是依然保留了cpu的执行资格，所以有可能cpu下次进行线程调度还会让这个线程获取到执行权继续执行  
- join ()执行后线程进入阻塞状态，例如在线程B中调用线程A的join ()，那线程B会进入到阻塞队列，直到线程A结束或中断线程

## 谈谈对线程安全的理解
- 线程安全指的是，在堆内存中的数据由于可以被任何线程访问到，在没有限制的情况下存在被意外修改的风险
- 当多个线程访问一个对象时，如果不用进行额外的

## Thread和Runable的区别
- Runnable的实现方式是实现其接口即可
- Thread的实现方式是继承其类
- Runnable接口支持多继承，但基本上用不到
- Thread实现了Runnable接口并进行了扩展，而Thread和Runnable的实质是实现的关系，不是同类东西，所以Runnable或Thread本身没有可比性。

## 说说对守护线程的理解?
守护线程:为所有非守护线程提供服务的线程;任何一个守护线程都是整个VM中所有非守护线程的保姆;当所有的用户进程结束了之后就会中断守护线程。注意:由于守护线程的终止是自身无法控制的，因此千万不要把IO、File等重要操作逻辑分配给它;因为它可能突然就中断了;  
- thread.setDaemon(true)必须在thread.start()之前设置，否则会跑出一个llegalThreadStateException异常。你不能把正在运行的常规线程设置为守护线程。
- 在Daemon线程中产生的新线程也是Daemon的。
- 守护线程不能用于去访问固有资源，比如读写操作或者计算逻辑。因为它会在任何时候甚至在一个操作的中间发生中断。

## ThreadLocal使用场景和原理
- 每一个Thread对象均含有一个ThreadLoca1Map类型的成员变量threadLocals，它存储本线程中所有ThreadLocal对象及其对应的值
- ThreadLoca1Map由一个个Entry对象构成
- Entry继承自weakReference<ThreadLocal<?>>，一个Entry由 ThreadLoca1对象和object构成。由此可见，Entry的key是ThreadLocal对象，并且是一个弱引用。当没指向key的强引用后，该key就会被垃圾收集器回收
- 当执行set方法时，ThreadLocal首先会获取当前线程对象，然后获取当前线程的ThreadLocalMap对象。再以当前ThreadLocal对象为key，将值存储进ThreadLocalMap对象中。
- get方法执行过程类似。ThreadLocal首先会获取当前线程对象，然后获取当前线程的ThreadLocalMap对象。再以当前ThreadLocal对象为key，获取对应的value。
- 由于每一条线程均含有各自私有的ThreadLocalMap容器，这些容器相互独立互不影响，因此不会存在线程安全性问题，从而也无需使用同步机制来保证多条线程访问容器的互斥性。 

![image](https://user-images.githubusercontent.com/92672384/153321258-c3cb3714-bcc6-40e7-a375-61ee739d3b42.png)
