---
layout: post
title: "Optional in Java"
permalink: blog/java-optional/
tags: [JVM]
---

`Optional` was first used in Java 8. It is similar to the Scala `Option` but less powerful. It can be used to represent null values in our application in a type safe way during compile time. In short, it acts as a container which could possibly contain null values. Let's look at examples to understand better.

Creating an empty `Option`

{% highlight java %}

Optional<String> value = Optional.empty();

{% endhighlight %}

The `Optional` class is available in the `import java.util.Optional` class. 

We can then test if this value is set using the `isPresent` method.

{% highlight java %}

 Optional<String> value = Optional.empty();

 System.out.println(value.isPresent());

{% endhighlight %}

would print false.

But if we change and assign a value then it becomes true.

{% highlight java %}

Optional<String> value = Optional.empty();

System.out.println(value.isPresent());

value = Optional.of("Testing");

System.out.println(value.isPresent());

{% endhighlight %}

The second sysout would print true. We should use `Optional.of` to wrap values into the `Optional` class.

But this does not prevent us from null. Something like below will cause `NullPointerException`.

{% highlight java %}

Optional<Integer> opt = null;

System.out.println(opt.isPresent());

{% endhighlight %}

For values which could be null then we can use the `ofNullable` method.

{% highlight java %}

String obj = null;

Optional<String> value = Optional.ofNullable(obj);

System.out.println(value.isPresent());

{% endhighlight %}

Usually we would want some kind of action to be done if it is null or the value is present. We can do that by using the method `ifPresent`.

{% highlight java %}

String obj = "testing";

Optional<String> value = Optional.ofNullable(obj);

value.ifPresent( a -> System.out.println(a));

{% endhighlight %}

The `ifPresent` method takes in a lambda which was the void return type. 

A good practice is to set an alternate if we encounter a null.

{% highlight java %}

String obj = null;

String value = Optional.ofNullable(obj).orElse("John");

System.out.println(value);

{% endhighlight %}

Notice that the type of value is a well formed String. This is helpful in situations where we want a reasonable default.

We can use `orElseGet` in place of `orElse` which has the capability to use a function instead of a value.

{% highlight java %}

String obj = null;

String value = Optional.ofNullable(obj).orElseGet(() -> "John");

System.out.println(value);

{% endhighlight %}

If we want to take action based on the type, then we can use the `ifPresent` as mentioned above. But what if we want to have two different actions i.e one based if value is present and the other if it is not?

Let's define a simple method which returns the String if its greater than 0 length else returns Illegal value as the String.

{% highlight java %}

public String calculateLength(String value){
        if (value.length()>0){
            return value;
        }
        else
            return "Illegal value";
    }

{% endhighlight %}

Then we write another method on top of this which check if it is not null then call `calculateLength` else just return null.

{% highlight java %}

 public String parseName(Optional<String> name){
        return name
                .map(this::calculateLength)
                .orElse("Null value");
    }

{% endhighlight %}

Now we can call this with three different states

{% highlight java %}

public static void main(String[] args) {

        String obj = null;

        Example example = new Example();

        Optional value = Optional.ofNullable(obj);

        System.out.println(example.parseName(value));


    }


    public String parseName(Optional<String> name){
        return name
                .map(this::calculateLength)
                .orElse("Null value");
    }

    public String calculateLength(String value){
        if (value.length()>0){
            return value;
        }
        else
            return "Illegal value";
    }

{% endhighlight %}

So we can call this with `null` then it prints "Null value". If the String is of zero length then we print "Illegal value" or else if the String is greater than 0 length and not null it prints the String itself.

There are other powerful higher order functions such as `flatMap` etc., which can be extended and used. But this should be sufficient to use `Optional` as mainstream in Java.