---
description: 因为课程需要最近深入看了下Java多线程，这里记录下遇到的问题
---

# This.getName\(\) vs. Thread.currentThread.getName\(\)

例子就用《Java多线程核心编程技术》里面的

```java
//CounterOperate.java
public class CountOperate extends Thread {
    public CountOperate() {
        System.out.println("constructor of CounterOperate---begin");
        System.out.println("Thread.currentThread().getName() = " + Thread.currentThread().getName());
        System.out.println("Thread.currentThread().isAlive() = " + Thread.currentThread().isAlive());
        System.out.println("this.getName()=" + this.getName());
        System.out.println("this.isAlive()=" + this.isAlive());
        System.out.println("constructor of CounterOperate---end");
    }

    @Override
    public void run() {
        System.out.println("run in CounterOperate--begin");
        System.out.println("Thread.currentThread().getName() = " + Thread.currentThread().getName());
        System.out.println("Thread.currentThread().isAlive() = " + Thread.currentThread().isAlive());
        System.out.println("this.getName()=" + this.getName());
        System.out.println("this.isAlive()=" + this.isAlive());
        System.out.println("run in CounterOperate--end");
    }
}
```

##  第一个例子，直接用instance start

```java
public class Run{
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Thread.currentThread().getName() = " + Thread.currentThread().getName());
        System.out.println("Thread.currentThread().isAlive() = " + Thread.currentThread().isAlive());
        CountOperate c= new CountOperate();
        c.()
    }
}
```

 直接new继承了Thread的类然后运行，结果如下，每一部分分析

```java
Thread.currentThread().getName() = main
Thread.currentThread().isAlive() = true
constructor of CounterOperate---begin
Thread.currentThread().getName() = main
Thread.currentThread().isAlive() = true
this.getName()=Thread-0
this.isAlive()=false
constructor of CounterOperate---end
run in CounterOperate--begin
Thread.currentThread().getName() = Thread-0
Thread.currentThread().isAlive() = true
this.getName()=Thread-0
this.isAlive()=true
run in CounterOperate--end
```

当前是在main这个线程中执行，alive自然也是true

```java
Thread.currentThread().getName() = main
Thread.currentThread().isAlive() = true
```

在**main这个线程**中进入了自定义类的构造函数所以current thread是main，并且alive，下面的this指向的是自定义线程类new出来的instance，当然这个线程还没有调用start方法alive是false

```java
constructor of CounterOperate---begin
Thread.currentThread().getName() = main
Thread.currentThread().isAlive() = true
this.getName()=Thread-0
this.isAlive()=false
constructor of CounterOperate---end
```

调用s.start后自定义的**线程c（Thread-0）开始执行**run\(\)中的部分所以current thread是Thread0，此时current thread和this是指向同一个instance的

```java
run in CounterOperate--begin
Thread.currentThread().getName() = Thread-0
Thread.currentThread().isAlive() = true
this.getName()=Thread-0
this.isAlive()=true
run in CounterOperate--end
```

## 第二个例子，通过thread构建新线程

```java
public class Run{
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Thread.currentThread().getName() = " + Thread.currentThread().getName());
        System.out.println("Thread.currentThread().isAlive() = " + Thread.currentThread().isAlive());
        CountOperate c= new CountOperate();
        Thread newThread = new Thread(c);
        newThread.start();
    }
}
```

 结果如下

```java
Thread.currentThread().getName() = main
Thread.currentThread().isAlive() = true
constructor of CounterOperate---begin
Thread.currentThread().getName() = main
Thread.currentThread().isAlive() = true
this.getName()=Thread-0
this.isAlive()=false
constructor of CounterOperate---end
run in CounterOperate--begin
Thread.currentThread().getName() = Thread-1
Thread.currentThread().isAlive() = true
this.getName()=Thread-0
this.isAlive()=false
run in CounterOperate--end
```

这里只分析不同的地方

```java
run in CounterOperate--begin
Thread.currentThread().getName() = Thread-1
Thread.currentThread().isAlive() = true
this.getName()=Thread-0
this.isAlive()=false
run in CounterOperate--end
```

明确一下，这个例子中一共有三个线程，main，thread0（c）和thread1（newThread），是newThread调用的start\(\)开启了新的线程，而c中的run是在newThread开启的这个线程中执行的所以current thread是thread1，本来想看看newThread是如何把c中的run包裹进去并且执行的，唯一知道的是Thread构造器中把runnable的target给了自己的target field，执行过程一路点下去点到了👇这个本地方法看不到了

```java
private native void start0();
```

 

