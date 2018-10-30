---
layout: post
title: "Threading in Java"
permalink: blog/java-threading/
tags: [JVM]
---

Threading in Java
-----------------

Threads in Java are some of the most basic forms of concurrency. They have been in existence for a very long time.

[Concurrency vs Parallelism](https://www.youtube.com/watch?v=cN_DpYBzKso())

Below we have code examples for all threading functionality in Java.

A way to create a thread in Java is using either extending the `Thread` class or by implementing the `Runnable` interface.

> Fun fact - The `Thread` class by itself implements `Runnable`

{% highlight java %}

public class ThreadImpl extends Thread {

    @Override
    public void run() {
        System.out.println("From class :" +ThreadImpl.class.getName());
    }
}

{% endhighlight %}

This can be called from a main method.

{% highlight java %}

public class Main {

    public static void main(String[] args) {

        new ThreadImpl().start();

    }

}

{% endhighlight %}

We can also make it print the current thread, so that we will know in fact it is a different thread.

{% highlight java %}

public class ThreadImpl extends Thread {

    @Override
    public void run() {
        System.out.println("From thread:" +Thread.currentThread().getName());
        System.out.println("From class :" +ThreadImpl.class.getName());
    }
}

{% endhighlight %}

This will print `From thread:Thread-0`. 

Creating threads using the `Runnable` interface is also very similar.

Setting Names
-------------

We can give names to threads. First we have to provide that facility in our `ThreadImpl` class itself.

{% highlight java %}

public class ThreadImpl extends Thread {

    public ThreadImpl(String customThread) {
        super(customThread);
    }

    @Override
    public void run() {
        System.out.println("From thread:" +Thread.currentThread().getName());
        System.out.println("From class :" +ThreadImpl.class.getName());
    }
}

{% endhighlight %}

And then from the main method, we can create a named thread like below.

{% highlight java %}

new ThreadImpl("CustomThread").start();

{% endhighlight %}

We can even set the name after they have been created.

{% highlight java %}

t1.setName("NameChanged");

{% endhighlight %}

More detailed tutorials can be found [here](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html). As you can see, threads are very low level constructs and are often used as building blocks to achieve higher level abstractions.

Direct OS mapping
-----------------

A JVM thread directly maps to the OS thread. This it is called as a low level construct. Any thread creation, destruction and its lifecycle is managed directly by the OS and is called from JVM to the OS directly.

Here is a good article by Unmesh Joshi which beautifully explains how it maps - [How Java thread maps to OS thread?
](https://medium.com/@unmeshvjoshi/how-java-thread-maps-to-os-thread-e280a9fb2e06)

Sleep
-----

We can basically suspend a thread by using `Thread.sleep`.

{% highlight java %}

Thread.sleep(1000);

{% endhighlight %}

This makes the current thread sleep for 1 second i.e 1000 milliseconds. Its almost never a good idea to make thread sleep because it basically prevents the thread from doing any other work. Remember that threads are costly to create.

Interrupts
----------

A thread's execution can be interrupted from an another thread. When this happens, the thread being interrupted throws an exception.

Let's setup our thread class to do some long running work i.e something like `Thread.sleep`.

{% highlight java %}

@Override
    public void run() {
        try {
            Thread.sleep(5000);
            System.out.println("From thread:" + Thread.currentThread().getName());
            System.out.println("From class :" + ThreadImpl.class.getName());
        }

        catch (InterruptedException e){
            System.out.println("Interrupted");
        }
    } 

{% endhighlight %}

Next, we can start this thread and then throw an interrupt.

{% highlight java %}

Thread t1 = new ThreadImpl("CustomThread");

t1.setName("NameChanged");

t1.start();

t1.interrupt();

{% endhighlight %}

This will print the message Interrupted and the program will exit. 

Further Topics
--------------

Below are basic concepts regarding concurrent programming

- Race conditions
- Deadlock
- Locks managed by JVM i.e synchronize keyword on methods/objects - Structured locks
- Manual locking/Unstructured locks - Read/Write lock, Re-entrant lock etc.,
- Critical section/Mutual exclusion
- Object based isolation
- Atomic variables - Implemented in Hardware itself natively.
- Actors
- Optimistic concurrency

Critical sections, Mutual exclusion, Atomic variables are higher level abstractions i.e they use unstructured locks behind the scenes without us having to worry about lower level semantics.

All of the concurrency primitives evolved to higher level abstractions so that it is easier to program in and also provide more fine grained control over locks and hence enabling better performance.




