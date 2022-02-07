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
