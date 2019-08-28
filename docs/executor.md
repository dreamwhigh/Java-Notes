```
package com.interview.javabasic.thread;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorDemo {
	public static void main(String[] args) {
		ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
		System.out.println("cachedThreadPool");
		for(int i = 0;i < 3;i++) {
			cachedThreadPool.execute(new MyRunnable("ctp"+i));
			}
		cachedThreadPool.shutdown();
		
		
		ExecutorService fixedThreadPool = Executors.newFixedThreadPool(2);
		System.out.println("fixedThreadPool");
		for(int i = 0;i < 3;i++) {
			fixedThreadPool.execute(new MyRunnable("ftp"+i));
			}
		fixedThreadPool.shutdown();
		
		ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();
		System.out.println("singleThreadExecutor");
		for(int i = 0;i < 3;i++) {
			singleThreadExecutor.execute(new MyRunnable("ste"+i));
			}
		singleThreadExecutor.shutdown();
		
		ExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(2);
		System.out.println("scheduledThreadPool");
		for(int i = 0;i < 3;i++) {
			scheduledThreadPool.execute(new MyRunnable("stp"+i));
			}
		scheduledThreadPool.shutdown();		
		
	}
}

```

```java
//MyRunnable
public class MyRunnable implements Runnable {
	private String name;
	public MyRunnable(String name) {
		this.name = name;
	}
	@Override
	public void run(){
		for(int j = 0;j < 3;j++) {
			System.out.println("Thread start:"+this.name+",j="+j);
		}
	}
}
```



```java
public class ExecutorDemo {
	public static void main(String[] args) {
		ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
		System.out.println("cachedThreadPool");
		for(int i = 0;i < 3;i++) {
			cachedThreadPool.execute(new MyRunnable("ctp"+i));
			}
		cachedThreadPool.shutdown();
	}
}

//结果
cachedThreadPool
Thread start:ctp1,j=0
Thread start:ctp2,j=0
Thread start:ctp0,j=0
Thread start:ctp2,j=1
Thread start:ctp1,j=1
Thread start:ctp2,j=2
Thread start:ctp0,j=1
Thread start:ctp1,j=2
Thread start:ctp0,j=2

```

