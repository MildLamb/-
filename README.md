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
