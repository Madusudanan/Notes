---
layout: post
title: "Variance in Java"
permalink: blog/java-variance/
tags: [JVM]
---

Variance in Java
----------------

The concept of variance originates in mathematics. Let's not go into the theory, but rather we can understand with the help of examples.

Let's create a class called `Vehicle`

{% highlight java %}

public class Vehicle {
}

{% endhighlight %}

Let's create another class called `Car` which extends `Vehicle`.

{% highlight java %}

public class Car extends Vehicle {
}

{% endhighlight %}

Now, another class called `Tiago` which extends `Car`.

{% highlight java %}

public class Tiago extends Car {
}

{% endhighlight %}

So far so good. Its clean OOP that we have seen over the course of years. Let's create another class called `Volvo` which extends `Vehicle` directly instead of some intermediate class like `Bus`. 

{% highlight java %}

public class Volvo extends Vehicle {
}

{% endhighlight %}

We will use the class `Volvo` to understand the concept of variance later. For now, just assume it is a badly designed class but technically a correct one as a `Volvo` is a type of a `Vehicle`.

Next, let's create a `Main` class and create some instances of the above defined classes.

{% highlight java %}

public class Main {

    public static void main(String[] args) {

        Vehicle vehicle = new Car();

    }

}

{% endhighlight %}

We can do this since `Car` is a sub class of `Vehicle`. This should not be new to you. It is just basic polymorphism.

Next create a simple method 

{% highlight java %}

 public static void f(Vehicle v){

 }

{% endhighlight %}

We can now pass values to this method using our polymorphic class types. For example,

{% highlight java %}

 Vehicle vehicle = new Car();
 f(vehicle);
 f(new Tiago());
 f(new Vehicle());
 f(new Volvo());

{% endhighlight %}

You can even make sure to run this code and it will work. This is called `Co-variance` i.e the Java compiler/run time allows a variant of the root class i.e `Vehicle` to work in this way.

Now, lets create a `List` that will hold a list of `Car` objects.

{% highlight java %}

List<Car> cars = new ArrayList<>();

{% endhighlight %}

Next, we will create a method called `fList` which will take a List of `Vehicle` objects.

{% highlight java %}

 public static void fList(List<Vehicle> vehicle){
 }

{% endhighlight %}

We should be able to call this `fList` method with the `cars` list that we create above.

{% highlight java %}

fList(cars);

{% endhighlight %}

But, it will create an error.

![Error - In variant](/images/variance_error.png)

This is because, collections in Java are not co-variant. In fact they are in-variant. The result would be the same with any collection under the `java.util.List` package. There is a reason why collections are invariant, we will explore that later. But let's try to use the same with `Arrays` which is technically not part of Java Collection.

Let's create a method which is similar to `fList` above.

{% highlight java %}

public static void fArray(Vehicle[] vehicles){
}

{% endhighlight %}

If we call it using an array of `Car` objects. It would work.

{% highlight java %}

Vehicle[] v = new Car[10];
fArray(v);

{% endhighlight %}

The compiler would not complain and the code would just work. In this particular use case, it feels as if arrays are more powerful in this case since they are co-variant. The answer is no. Let's understand that with more examples.

We all know the contract of the `List` definition below.

{% highlight java %}

List<Car> cars = new ArrayList<>(); 

{% endhighlight %}

The `cars` list should only contain objects which are of the `Car` hierarchy.

But inside the `fList` method we can very well add any type of the `Vehicle` class.

{% highlight java %}

 public static void fList(List<Vehicle> vehicle){
    vehicle.add(new Volvo());
 }

{% endhighlight %}

This is not correct as per our definition. A `Volvo` is not a sub class of car, but we can add it our list which is wrong. This is why collections are invariant. We can still do the same mistake in arrays.

{% highlight java %}

public static void fArray(Vehicle[] vehicles){
        vehicles[0] = new Volvo();
}

{% endhighlight %}

The main difference to notice here is the compiler does not allow the list example to compile but happily allows the array example to compile.  Also, when we try to run the array example a `RuntimeException` is thrown.

{% highlight java %}

Exception in thread "main" java.lang.ArrayStoreException: variance.Volvo
	at variance.Main.fArray(Main.java:35)
	at variance.Main.main(Main.java:20)

{% endhighlight %}

In this case, the error comes in Runtime which is bad. A compile time exception is far better than having it at runtime.

This is because that generics do not exist at the JVM runtime level. They only exist at compile time. To explain it further, there is no `Car` type at the JVM level. All it knows is that it is a type of `Object`. This is something called type erasure which will explore in a later dedicated tutorial.

We can bring a little bit of co-variance our `fList` method by changing its syntax.

{% highlight java %}

public static void fList(List<? extends Vehicle> vehicle){
}

{% endhighlight %}

Now what we mean by our signature is that it accepts all types of `List` of objects which extends the `Vehicle` object.

We can then pass in our cars list as initially intended to do.

{% highlight java %}

List<Car> cars = new ArrayList<>();
fList(cars);

{% endhighlight %}

But we cannot do something like below.

{% highlight java %}

public static void fList(List<? extends Vehicle> vehicle){
    vehicle.add(new Volvo());
}

{% endhighlight %}

![Co variance error](/images/co-variance-error.png)

This is a limitation which we will explore later and it is due to type erasure. For now, we can assume that we can read from the passed in list of objects but cannot write to it in a co-variant manner.

So from the above examples, it should be self explanatory on Co-variance and Invariant i.e no variance. There is also something called contra-variant i.e the opposite of covariance.

Let's create a method similar to `fList` which accepts a list of `Tiago` cars.

{% highlight java %}

public static void fContraList(List<Tiago> cars){
}

{% endhighlight %}

Now we cannot call this method with a list of `Car` objects since List by default is in-variant. But what if we want to do this? We can change the method definition such as below.

{% highlight java %}

public static void fContraList(List<? super Tiago> cars){
}

{% endhighlight %}

Which means that it accepts a list of Objects that are some super class of `Tiago`. This has one disadvantage though. We cannot do something like below.

![Contra variance error](/images/contra-variance-error.png)

We lose the type information here since it is not possible to know at compile time at what could be the class which could be a super class of `Tiago`. We should use type casting here(bad practice) to achieve what we need.

{% highlight java %}

public static void fContraList(List<? super Tiago> cars){
    Car car = (Car) cars.get(0);
}

{% endhighlight %}

To summarize

- Introduction to variance
- Co-variant and In-variant and usage in Java collections and normal code
- Contra-variance

It is very important to understand variance as it used in Akka Streams pretty heavily.

Reference - [Covariance, Invariance and Contravariance explained in plain English?](https://stackoverflow.com/questions/8481301/covariance-invariance-and-contravariance-explained-in-plain-english)