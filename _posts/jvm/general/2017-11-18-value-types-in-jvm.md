---
layout: post
title: "JVM Value Types & Project Valhalla"
permalink: blog/value-types-in-jvm-and-project-valhalla/
tags: [JVM]
---

Value Types
-----------

<h3><b><a name = "Problem" class="inter-header">The Problem</a></b></h3>

In Java, there are two different types i.e primitive types and reference types. Primitive types are represented 
[here](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html){:target="_blank"}

If you have programmed using [Generics](https://docs.oracle.com/javase/tutorial/java/generics/index.html){:target="_blank"} then you would have done
[boxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html){:target="_blank"}. 

{% highlight java %}

ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);

{% endhighlight %}

So everything is fine right? What is the problem?. Well boxing is not really necessary and takes a performance hit. Primitive types
are directly mapped to the appropriate space in assembly/instruction set so its clean there. But they cannot directly be represented
in generics since they operate on Objects. This difference of Objects vs Primitives is actually unnecessary.

Scala handles this difference and has a much better 
[type system](https://madusudanan.com/blog/scala-tutorials-part-2-type-inference-in-scala/){:target="_blank"}. But it runs on the JVM, so even
though the compiler takes care of the drudgery work the run time however deals it poorly.

Generics in Java throws away the type information at compile time which is called 
[type erasure](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html){:target="_blank"} which makes the job simple for the runtime
but also loses valuable type information which could have been used for better compile time errors. From a programmer perspective, the 
`ArrayList` data structure at run time converts all of the types to the root type i.e the `Object` type. 

Type erasure despite all of its disadvantages allow other languages to be run on the JVM. But then again, there is a lot of room to improve.

<h3><b><a name = "Solution" class="inter-header">Solution</a></b></h3>

Languages such as Scala,Kotlin already solve this to a certain extent for the developer. But still the tax on the runtime is not removed.

Enter [project valhalla](http://openjdk.java.net/projects/valhalla/){:target="_blank"}. This project started in 2015 to provide value
types as part of the JVM bytecode itself. They have considered the approaches of C++, C# and are trying to build upon it.

The below video goes into detail of the implementation detail.

[![Adventures on the Road to Valhalla](https://img.youtube.com/vi/uNgAFSUXuwc/0.jpg)](https://www.youtube.com/watch?v=uNgAFSUXuwc){:target="_blank"}

This takes time because it has to be backwards compatible with older java codebases and is set to make landfall in JDK 10 (Hopefully).

Also read a blog on the same subject by [Jesper DJ](http://www.jesperdj.com/2015/10/04/project-valhalla-value-types/){:target="_blank"}.


