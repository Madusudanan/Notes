---
layout: post
title: "Build tools for Java and JVM languages"
permalink: blog/build-tools-for-java-and-jvm-languages/
tags: [JVM]
---

Build tools
-----------

Whatever language you choose, a build tool is essential to handle all of the work related to build, dependency and package management. C++ has make,
Java has Ant,Maven,Gradle etc.,

It can greatly improve developer productivity even in the case of a single developer project.

If we go [back in history](https://technologyconversations.com/2014/06/18/build-tools/){:target="_blank"}, it all began with 
[Apache Ant](http://ant.apache.org/){:target="_blank"} integrated with [Apache Ivy](http://ant.apache.org/ivy/){:target="_blank"}. It was fine
for some time, but as application needs and libraries began to grow it was cumbersome to manage with Ant. An example would be that Ant does not
handle dependant Jars by itself. Choosing a build tool is a complicated task by itself, mainly because there are a lot of functionalities.

Maven is a build tool that is mainly used in the Java land. It is much more than a build tool. It does project management stuff as well. 

So in general, maven is built based on Apache Ant and Ivy. Gradle came out since many people felt that XML based configuration was verbose and hence they went for a DSL based config. Recently, gradle moved from Groovy based DSL to Kotlin based DSL. SBT is another build tool for Scala based apps.

Build tool selection criteria

- Start with Maven, pretty standard and has very wide tool support and integration
- Choose Gradle if the team feels XML is pretty verbose(The cool kids use gradle btw)
- Choose SBT in case of Scala, but keep in mind that Scala based tools have received a lot of criticism recently. So keep an eye on [Maven based build for Scala](https://github.com/Madusudanan/MavenScala){:target="_blank"} as a fallback mechanism in case things [go south with SBT](http://www.lihaoyi.com/post/SowhatswrongwithSBT.html){:target="_blank"}.

Whatever you choose, make sure that the developer has least overhead to maintain it. Build tools are meant to make the job easier for the developer rather than the other way around.

Also keep in mind that once you setup a base project, it is not going to change frequently. So certain things like readability, verbosity matters less when compared to actual language source code.