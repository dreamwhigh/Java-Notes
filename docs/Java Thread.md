---
\typora-copy-images-to: pics
---

# Java Thread

## 进程和线程

[java 高并发面试题](https://blog.csdn.net/u012998254/article/details/79400549)

进程是一个实体。每一个进程都有它自己的地址空间，一般情况下，包括文本区域（text region）、数据区域（data region）和堆栈（stack region）。文本区域存储处理器执行的代码；数据区域存储变量和进程执行期间使用的动态分配的内存；堆栈区域存储着活动过程调用的指令和本地变量。
一个标准的线程由线程 ID，当前指令指针 (PC），寄存器集合和堆栈组成。另外，线程是进程中的一个实体，是被系统独立调度和分派的基本单位，线程自己不拥有系统资源，只拥有一点儿在运行中必不可少的资源，但它可与同属一个进程的其它线程共享进程所拥有的全部资源。

区别

1. 地址空间：进程内的一个执行单元;进程至少有一个线程；它们共享进程的地址空间;而进程有自己独立的地址空间; 
2. 资源拥有：进程是资源分配和拥有的单位，同一个进程内的线程共享进程的资源。
3. 线程是处理器调度的基本单位，但进程不是。
4. 二者均可并发执行。

![1564636568104](E:\GitHub\CS-Learning\docs\java\pics\1564636568104.png)

![1564636668106](E:\GitHub\CS-Learning\docs\java\pics\1564636668106.png)

## start 和 run 方法

![1564624351517](E:\GitHub\CS-Learning\docs\java\pics\1564624351517.png)

start() 方法会调用 JVM_StartThread **创建一个新的子线程**，并通过 thread.run() 方法去调用 Thread.run() 方法。

- 调用 start() 方法会创建一个新的子线程并启动
- run() 方法只是 Thread 的一个普通方法的调用

## Thread 和 Runnable

- Thread 是实现了 Runnable 接口的类，使得 run 支持多线程
- 因类的单一继承原则，推荐多使用 Runnable 接口

### 创建多线程

#### 继承 Thread 类

```java
//MyThread 继承 Thread 类
public class MyThread extends Thread{
	private String name;
	public MyThread(String name) {
		this.name = name;
	}
	@Override
	public void run(){
		for(int i = 0;i < 10;i++) {
			System.out.println("Thread start:"+this.name+",i="+i);
		}
	}
}

public class ThreadDemo {
	public static void main(String[] args) {
		MyThread mt1 = new MyThread("Thread1");
		mt1.start();//继承 Thread 的类可直接调用 start()
	}
}
```

#### 实现 Runnable 接口

```java
public class MyRunnable implements Runnable {
	private String name;
	public MyRunnable(String name) {
		this.name = name;
	}
	@Override
	public void run(){
		for(int i = 0;i < 10;i++) {
			System.out.println("Thread start:"+this.name+",i="+i);
		}
	}
}

public class RunnableDemo {
	public static void main(String[] args) {
		MyRunnable mr1 = new MyRunnable("Runnable1");
		Thread t1 = new Thread(mr1);
		t1.start();
	}
}
```

#### 比较

- 如果一个类继承Thread，则不适合资源共享。但是如果实现了Runable接口的话，则很容易的实现资源共享。
- main函数，实例化线程对象也有所不同，
  extends Thread ：t.start();
  implements Runnable ： new Thread(t).start();
- 使用 Runnable，增加程序的健壮性，代码可以被多个线程共享，代码和数据独立
- 线程池只能放入实现 Runable 或 Callable 类线程，不能直接放入继承 Thread 的类

## 处理线程的返回值

### 主线程等待法

```java
public class CycleWait implements Runnable{
	private String value;
	public void run() {
		try {
			Thread.currentThread().sleep(5000);
		}catch (InterruptedException e){
			e.printStackTrace();
		}
		value = "we hava data now";		
	}
	
	public static void main(String[] args) throws InterruptedException {
		CycleWait cw = new CycleWait();
		Thread t = new Thread(cw);
		t.start();
        //当 value 赋值语句还未执行时，主线程等待，直至 value 不为空
		while(cw.value == null) {
            //主线程每次等待 100 ms
			Thread.currentThread().sleep(100);
		}
		System.out.println("value:" + cw.value);
	}
}
```

实现简单

需要自己实现等待逻辑，代码长

等待时间无法做出精准控制

### join() 阻塞法

```java
public static void main(String[] args) throws InterruptedException {
    CycleWait cw = new CycleWait();
    Thread t = new Thread(cw);
    t.start();
    t.join();//阻塞当前主线程，直至子线程处理完毕
    System.out.println("value:" + cw.value);
}
```

### Callable 接口

#### FutureTask 获取

```java
import java.util.concurrent.*;

//实现 Callable 接口
public class MyCallable implements Callable<String>{
	@Override
	//重写 Callable 中的  call() 方法
	public String call() throws Exception{
		String value = "test";
		System.out.println("Ready to work");
		Thread.currentThread().sleep(5000);
		System.out.println("task done");
		return value;
	}
}

public class FutureTaskDemo {
	public static void main(String[] args) throws InterruptedException, ExecutionException{
		FutureTask<String> task = new FutureTask<String> (new MyCallable());
		new Thread (task).start();
		if(!task.isDone()) {
			System.out.println("task has not finished, please wait!");
		}
		System.out.println("task return:" + task.get());
	}
}
```

#### 线程池获取

```java
import java.util.concurrent.*;
public class ThreadPoolDemo {
	public static void main(String[] args) {
		ExecutorService newCachedThreadPool = Executors.newCachedThreadPool();
		Future<String> future = newCachedThreadPool.submit(new MyCallable());
		if(!future.isDone()) {
			System.out.println("task has not finished, please wait!");
		}
		try {
			System.out.println(future.get());
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (ExecutionException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally {
			newCachedThreadPool.shutdown();
		}
	}
}
```

## 线程的状态

六个状态



## sleep() 和 wait()

### 基本差别

| 方法名  | 所属类    | 使用条件                                         |
| ------- | --------- | ------------------------------------------------ |
| sleep() | Thread 类 | 在任何地方均可使用                               |
| wait()  | Object 类 | 只能在 synchronized 方法或 synchronized 块中使用 |

### 本质区别

- Thread.sleep 只会让出 CPU，不会导致锁行为的改变
- Object.wait 不仅会让出 CPU，还会释放已经占有的同步资源锁

```java
//wait() 在前，sleep() 在后
public class WaitSleepDemo {
	public static void main(String[] args) {
		final Object lock = new Object();
		new Thread (new Runnable() {
			@Override
			public void run() {
				System.out.println("thread A is waiting to get lock");
				synchronized(lock) {
					try {
					System.out.println("thread A get lock");
					Thread.sleep(20);
					System.out.println("thread A do wait method");
					lock.wait(1000);//wait 期间会释放同步锁和CPU
					System.out.println("thread A is done");
				}catch(InterruptedException e) {
					e.printStackTrace();
				}				
			}
		}
	}).start();
		try {
			Thread.sleep(10);
		}catch(InterruptedException e) {
			e.printStackTrace();
		}
		new Thread (new Runnable() {
			@Override
			public void run() {
				System.out.println("thread B is waiting to get lock");
				synchronized(lock) {
					try {
					System.out.println("thread B is sleeping");
					Thread.sleep(10);
					System.out.println("thread B is done");
				}catch(InterruptedException e) {
					e.printStackTrace();
				}				
			}
		}
	}).start();
		
}
	
}
```

结果

```
thread A is waiting to get lock
thread A get lock
thread A do wait method
thread B is waiting to get lock
thread B is sleeping
thread B is done
thread A is done
```



```java
//sleep() 在前，wait() 在后
public class WaitSleepDemo {
	public static void main(String[] args) {
		final Object lock = new Object();
		new Thread (new Runnable() {
			@Override
			public void run() {
				System.out.println("thread A is waiting to get lock");
				synchronized(lock) {
					try {
					System.out.println("thread A get lock");
					Thread.sleep(20);
					System.out.println("thread A do sleep method");
					Thread.sleep(1000);//sleep() 不会释放锁
					System.out.println("thread A is done");
				}catch(InterruptedException e) {
					e.printStackTrace();
				}				
			}
		}
	}).start();
		//保证线程 A 先执行
		try {
			Thread.sleep(10);
		}catch(InterruptedException e) {
			e.printStackTrace();
		}
		new Thread (new Runnable() {
			@Override
			public void run() {
				System.out.println("thread B is waiting to get lock");
				synchronized(lock) {
					try {
					System.out.println("thread B get lock");		
					System.out.println("thread B do wait method");
					lock.wait(10);
					System.out.println("thread B is done");
				}catch(InterruptedException e) {
					e.printStackTrace();
				}				
			}
		}
	}).start();
		
}
	
}
```

结果

```
thread A is waiting to get lock
thread A get lock
thread A do sleep method
thread B is waiting to get lock
thread A is done
thread B get lock
thread B do wait method
thread B is done
```

### notify 和 notifyAll

notify 和 notifyAll 用于唤醒 wait(）

#### 锁池

假设线程 A 已经拥有了某个对象（不是类）的锁，而其它的线程想要调用这个对象的某个 synchronized 方法（或者 synchronized 块），由于这些线程在进入对象的 synchronized 方法之前必须先获得该对象的锁的拥有权，但是该对象的锁目前正被线程 A 拥有，所以这些线程就进入了该对象的锁池中。

#### 等待池

假设一个线程 A 调用了某个对象的 wait() 方法，线程 A 就会释放该对象的锁（因为 wait() 方法必须出现在 synchronized 中，这样自然在执行 wait() 方法之前线程 A 就已经拥有了该对象的锁），同时线程 A 就进入到了该对象的等待池中。

如果另外的一个线程调用了相同对象的 notifyAll() 方法，那么处于该对象的等待池中的线程就会全部进入该对象的锁池中，准备争夺锁的拥有权。如果另外的一个线程调用了相同对象的 notify() 方法，那么仅仅有一个处于该对象的等待池中的线程(随机)会进入该对象的锁池。

#### 区别

- notifyAll 会让**所有**处于等待池的线程全部进入锁池去竞争获取锁的机会
- notify 只会**随机选取一个**处于等待池中的线程进入锁池去竞争获取锁的机会



## yield

yield() 使当前运行线程 A 从执行状态（运行状态）变为可运行状态（就绪状态），以允许具有相同优先级的其他线程获得运行机会。因此，使用 yield() 的目的是让相同优先级的线程之间能适当的轮转执行。

但是，实际中无法保证 yield() 达到让步目的，因为cpu会从众多的可执行态里选择，让步的线程还有可能被线程调度程序再次选中。也就是说，线程 A 还是有可能会被再次执行到的，并不是说一定会执行其他线程。

yield() 从未导致线程转到等待/睡眠/阻塞状态。在大多数情况下，yield()将导致线程从运行状态转到可运行状态，但有可能没有效果。

## 中断线程

### interrupt() 方法

![1564661919274](E:\GitHub\CS-Learning\docs\java\pics\1564661919274.png)

![1564661871193](E:\GitHub\CS-Learning\docs\java\pics\1564661871193.png)

## 线程状态图



![1564662133522](E:\GitHub\CS-Learning\docs\java\pics\1564662133522.png)