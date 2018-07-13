---
layout: post
title: "Type Inference"
permalink: blog/type-inference/
tags: [JVM]
---

<h3><b><a name = "TypeInfo" class="inter-header">Type Inference Intro</a></b></h3>

[Type inference](https://en.wikipedia.org/wiki/Type_inference){:target="_blank"} is a concept where we need not explicitly mention the type
of a variable although we can which is known as Type annotation or Type ascription.

Many people get confused as type inference is dynamic typing, but its very well within the static typing land. The complain against static typing
was it is unnecessary in situations to force types of variables. Let's take the below example.

{% highlight java %}

 public static int getLengthOfString(String input){
        return input.length();
    }

{% endhighlight %}

The return type is obvious here. We can do better in Scala.

{% highlight scala %}

def getLengthOfString(input:String) = {
    input.length
}

{% endhighlight %}

The output type gets referred to `Int` here. Can be visualized better using the Scala REPL.

![String length example REPL](/images/string-length.png)

The type parameter of the input variable is necessary since Scala does not have full/hindley milner type inference. 

<h3><b><a name = "TypeInfoOtherLang" class="inter-header">Type Inference In Other Languages</a></b></h3>

### Haskell

Haskell since it is a pure functional language can have a function which calculates string length in which you need not annotate
both the function/method input parameter and the return type.

{% highlight haskell %}

let getLengthOfString input = length input

{% endhighlight %}

Can be then called using below syntax.

{% highlight haskell %}

getLengthOfString "Testing"

{% endhighlight %}

which would return 7.

Of course, Haskell can support HM style type inference since it does not do object oriented programming. The comparison between Scala and 
Haskell is more like a trade off and not an advantage.

### Kotlin

Let's take a look at an example in Kotlin.

{% highlight kotlin %}

fun getLengthOfString(input: String) = input.length
   
{% endhighlight %}

Calling this would like `getLengthOfString("Testing")` which would return 7. You can see that type annotation for the method parameter is required 
here as well. It is more like a limitation of the paradigm itself i.e OOP. Continous research is being done. Maybe we will have a OOP language
with HM type inference one day. But again I am no expert in compiler internals.


<h3><b><a name = "WhenToUse" class="inter-header">When To Use Type Inference</a></b></h3>

We can also explicitly annotate types as follows.

{% highlight scala %}


    def getLengthOfString(input:String) : Int = {
      input.length
    }

{% endhighlight %}

This is good practice if you are coding in public interface methods and it should not leave the developer guessing on how to call
the method. A good type system based on type inference has the advantages of both dynamic and static typing i.e it does not force you to 
annotate types everywhere and also giving evaluation at compile time rather than run time.


<h3><b><a name = "References" class="inter-header">References</a></b></h3>

Most of the below tutorials is advanced. Watch at your own convenience once you have mastered other stuff.

1) A good video on type systems and static typing.

[![Uncovering the Unknown: Principles of Type Inference](https://img.youtube.com/vi/fDTt_uo0F-g/0.jpg)](https://www.youtube.com/watch?v=fDTt_uo0F-g){:target="_blank"}
 
2) Type inference from a Haskell perspective

[![Introduction to Type Inference - Haskell](https://img.youtube.com/vi/il3gD7XMdmA/0.jpg)](https://www.youtube.com/watch?v=il3gD7XMdmA){:target="_blank"}

3) Type inference from a Scala perspective

[![Demystifying Type Inference - Scala](https://img.youtube.com/vi/bS2Tt7C6mDw/0.jpg)](https://www.youtube.com/watch?v=bS2Tt7C6mDw){:target="_blank"}

Type inference is coming in Java itself with [Project Amber](http://openjdk.java.net/projects/amber/){:target="_blank"}

Also read [Scala types/type inference in detail](https://madusudanan.com/blog/scala-tutorials-part-2-type-inference-in-scala/){:target="_blank"}

This note was about type inference in general, more language examples to follow.
