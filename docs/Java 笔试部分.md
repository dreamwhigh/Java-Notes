# 笔试

## 关键字

### static

static用来修饰类或类的成员，这时不需要创建实例就能访问（而且不能实例化），在被调用的时候自动实例化，且在内存中产生一个实例。当含有静态成员的非静态类实例化出对象后，这些对象公用这些静态成员，通过类名或对象名都能访问它们。



## 继承

### 定义

抽象方法没有方法体，只有声明。

```
abstract class xy
{
    abstract sum (int x, int y) { }//false
    abstract sum (int x, int y)；//true
}
```



### 匿名内部类

匿名内部类的创建格式为： **new 父类构造器（参数列表）|实现接口（）{**

​                                             **//匿名内部类的类体实现**

​                                        **}**

1. 使用匿名内部类时，必须继承一个类或实现一个接口
2. 匿名内部类由于没有名字，因此不能定义构造函数
3. 匿名内部类中不能含有静态成员变量和静态方法
4. 匿名内部类不能是抽象的，它必须要实现继承的类或者实现的接口的所有抽象方法。

### 构造方法

构造方法是：`public 类名` ，没有方法修饰符

```java
public class MyClass {
    long var;
    public void MyClass(long param) { var = param; }//是一个返回类型为 void 的普通方法，而不是构造方法
    public static void main(String[] args) {
        MyClass a, b;
        a =new MyClass();//true 默认的构造方法
        b =new MyClass(5);//false 没有带参数的构造器
        a.MyClass(5);//true 普通方法自然需要实例化对象然后去调用它
    }
}
```



```java
class Two{
    Byte x;
}
class PassO{
    public static void main(String[] args){
        PassO p=new PassO();
        p.start();
    }
    void start(){
        Two t=new Two();
        System.out.print(t.x+””);//null
        Two t2=fix(t);
        System.out.print(t.x+” ” +t2.x);//42 42
    }
    Two fix(Two tt){
        tt.x=42;
        return tt;
    }
}
output:null 42 42
```

<div align="center">
    <img scr="https://github.com/dreamwhigh/Java-Notes/blob/master/docs/pics/1.png?raw=true" width="400px">
</div>



### 正则表达式



## 容器

### HashMap



#### HashMap 和 HashTable



## 异常

<div align="center">
    <img src="https://github.com/dreamwhigh/Java-Notes/blob/master/docs/pics/异常分类.png?raw=true" width="400px">
</div>





1. 粉红色的是受检查的异常(checked exceptions)，其必须被 try{}catch语句块所捕获,或者在方法签名里通过throws子句声明。受检查的异常必须在编译时被捕捉处理，命名为 Checked Exception 是因为Java编译器要进行检查，Java虚拟机也要进行检查,以确保这个规则得到遵守。

2. 绿色的异常是运行时异常(runtime exceptions),需要程序员自己分析代码决定是否捕获和处理,比如 空指针,被0除...

3. 而声明为Error的，则属于严重错误，如系统崩溃、虚拟机错误、动态链接失败等，这些错误无法恢复或者不可能捕捉，将导致应用程序中断，Error不需要捕捉





## 并发

### Thread类

String getName()　　返回该线程的名称。

void setName(String name)　　改变线程名称，使之与参数 name 相同。

**int getPriority()** **返回线程的优先级。**

void setPriority(int newPriority) 　　更改线程的优先级。

boolean isDaemon() 　　测试该线程是否为守护线程。

void setDaemon(boolean on)　　将该线程标记为守护线程或用户线程。

static void sleep(long millis)

void interrupt()　　中断线程。

static void yield()　　暂停当前正在执行的线程对象，并执行其他线程。

void join()　　等待该线程终止。

void run()

void start()



A. 调用sleep()方法会让线程进入睡眠状态---睡眠指定的时间后再次执行；

B. 调用wait()方法会让线程进入等待状态 ----等待别的线程执行notify()或notifyAll()唤醒后继续执行；

C.调用start()方法会让线程进入就绪状态---得到CPU时间就执行线程；

D.run()方法是线程的具体逻辑方法，执行完，线程就结束。即线程被销毁



java用**监视器即锁**机制实现了进程之间的同步执行

### synchronized关键字和volatile关键字比较：

- volatile关键字是线程同步的轻量级实现，所以volatile性能肯定比synchronized关键字要好。但是volatile关键字只能用于变量而synchronized关键字可以修饰方法以及代码块。synchronized关键字在JavaSE1.6之后进行了主要包括为了减少获得锁和释放锁带来的性能消耗而引入的偏向锁和轻量级锁以及其它各种优化之后执行效率有了显著提升，实际开发中使用 synchronized 关键字的场景还是更多一些。
- 多线程访问volatile关键字不会发生阻塞，而synchronized关键字可能会发生阻塞
- volatile关键字能保证数据的可见性，但不能保证数据的原子性。synchronized关键字两者都能保证。
- volatile关键字主要用于解决变量在多个线程之间的可见性，而 synchronized关键字解决的是多个线程之间访问资源的同步性。
- **synchronized: 具有原子性，有序性和可见性**；（三个都有）
  **volatile：具有有序性和可见性（缺一个原子性）**

## JVM

对象存储在堆内存，引用变量存储在栈内存。栈内存指向堆内存

[Java](http://lib.csdn.net/base/17)把内存分成两种，一种叫做栈内存，一种叫做堆内存。



在函数中定义的一些基本类型的变量和对象的引用变量都是在函数的栈内存中分配。当在一段代码块中定义一个变量时，java就在栈中为这个变量分配内存空间，当超过变量的作用域后，java会自动释放掉为该变量分配的内存空间，该内存空间可以立刻被另作他用。



堆内存用于存放由new创建的对象和数组。在堆中分配的内存，由java虚拟机自动垃圾回收器来管理。在堆中产生了一个数组或者对象后，还可以在栈中定义一个特殊的变量，这个变量的取值等于数组或者对象在堆内存中的首地址，在栈中的这个特殊的变量就变成了数组或者对象的引用变量，以后就可以在程序中使用栈内存中的引用变量来访问堆中的数组或者对象，引用变量相当于为数组或者对象起的一个别名，或者代号。



引用变量是普通变量，定义时在栈中分配内存，引用变量在程序运行到作用域外释放。而数组＆对象本身在堆中分配，即使程序运行到使用new产生数组和对象的语句所在地代码块之外，数组和对象本身占用的堆内存也不会被释放，数组和对象在没有引用变量指向它的时候（比如先前的引用变量x=null时），才变成垃圾，不能再被使用，但是仍然占着内存，在随后的一个不确定的时间被垃圾回收器释放掉。这个也是java比较占内存的主要原因。

​      