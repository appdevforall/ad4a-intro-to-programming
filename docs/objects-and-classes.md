# Objects and classes

In this section we will talk further about objects, classes, and their packages.

Often we need to get the current time from a Java application. One way to do this is to use the `Instant` class, which comes with Java. The Java system comes with a _standard library_ of classes that do a variety of things you will want to utilize. As well, Java on the Android system comes with additional library classes for interacting with Android. We will explore some of that functionality later.

One problem is that the `Instant` class is not automatically available to your application - you have to _import_ it from another package. We talked about packages briefly in the prior lesson. All Java classes reside in packages. Some packages are available automatically, and some have to be explicitly imported.

We need to know how to import classes from other packages. But first, let's explore a new class that is already available.

## Exploring new classes

While your _Hello_ project is open in Code on the Go, start a terminal from the project menu, and then start the Java Shell `jshell` again.

```Java
.../CodeOnTheGoProjects/Hello $ jshell
|  Welcome to JShell -- Version 21.0.7
|  For an introduction type: /help intro

jshell>
```

Let's explore a class that is available automatically, the `StringBuilder` class in the package `java.lang`. That package is special because it is _always_ available. `StringBuilder` allows you to build up a string incrementally.

```Java
jshell> var builder = new StringBuilder()
builder ==>

jshell>
```

As we saw previously, the `new` operator creates a new _instance_ of a class, in this case a new instance of `StringBuilder`. The second line shows that the current value of the variable is the empty string. Let's append some new pieces.

```Java
jshell> builder.append("this")
$2 ==> this

jshell> builder.append("is")
$3 ==> thisis

jshell> builder.append("a")
$4 ==> thisisa

jshell> builder.append("test")
$5 ==> thisisatest

jshell> 
```

We can append other things to the `StringBuilder`, too, but we'll wait until we talk about numeric types before we explore that. Notice that the builder does _exactly_ what we said. It does not add any additional characters, such as spaces.

We can tell that `builder` is a `StringBuilder` instance by getting its class. Every class has a method called `getClass` that takes no arguments and returns the class of an instance object.

```Java
jshell> builder.getClass()
$6 ==> class java.lang.StringBuilder

jshell> 
```

We can convert that `StringBuilder` instance to a new variable that is a simple `String` by using the `toString()` method. Every class also supports `toString()`, and you can customize that behavior in your own classes.

```Java
jshell> var builtString = builder.toString()
builtString ==> "thisisatest"

jshell> builtString.getClass()
$8 ==> class java.lang.String

jshell> 
```

## Importing a class

We have used two classes, both from the `java.lang` package, `String` and `StringBuilder`. Let's now consider how to use the `Instant` class from the package `java.time`. First, let's verify that this class is not already available.

```Java
jshell> var now = new Instant()
|  Error:
|  cannot find symbol
|    symbol:   class Instant
|  var now = new Instant();
|                ^-----^

jshell> 
```

`Instant` is not available, but we can _import_ it like this, and try again.

**NOTE:** Missing section, adding classpath, then importing.

```Java
jshell> import java.time.Instant

jshell> var now = new Instant()
|  Error:
|  constructor Instant in class java.time.Instant cannot be applied to given types;
|    required: long,int
|    found:    no arguments
|    reason: actual and formal argument lists differ in length
|  var now = new Instant();
|            ^-----------^

jshell> 
```

Whoops! What went wrong? Well, you don't use `Instant` the same way you use `StringBuilder`. Instead of using `new`, you call a _class method_ to get the current time.

```Java
jshell> var now = Instant.now()
now ==> 2025-12-30T17:40:36.700070Z

jshell> now.getClass()
$11 ==> class java.time.Instant

jshell> 
```

The time shown on the third line is the same `String` value you'd get from `now.toString()`. The `jshell` automatically calls `toString()` to convert values to `String` when showing them.  The time is in a standard format showing the date, a `T` separator, and the time. The `Z` on the end means that the time is shown in _Universal Coordinated Time_, or UTC, roughly the time in Greenwich, England, but with no daylight savings time adjustment.

The class method `now()` returns a new `Instant` with the current time. As you can see, it is of the class we wanted. There are a lot of methods supplied by `Instant`, as you can see if you type `now.` and then Tab.

```Java
jshell> now.
adjustInto(        atOffset(          atZone(            compareTo(         
equals(            get(               getClass()         getEpochSecond()   
getLong(           getNano()          hashCode()         isAfter(           
isBefore(          isSupported(       minus(             minusMillis(       
minusNanos(        minusSeconds(      notify()           notifyAll()        
plus(              plusMillis(        plusNanos(         plusSeconds(       
query(             range(             toEpochMilli()     toString()         
truncatedTo(       until(             wait(              with(              
jshell> now.
```

Two that you might find interesting are `plusSeconds` and `toEpochMilli`.

```Java
jshell> now
now ==> 2025-12-30T17:40:36.700070Z

jshell> now.plusSeconds(60)
$13 ==> 2025-12-30T17:41:36.700070Z

jshell> now.toEpochMilli()
$14 ==> 1767116436700

jshell> 
```

You can see that adding 60 seconds using `plusSeconds` results in a new `Instant` that is one minute later. `toEpochMilli` returns the number of milliseconds from January 1, 1970, to the time stored in `now`. Internally, Java keeps track of time in that way, as milliseconds since that _epoch_ time.

## Exploring your console application

The `Hello` application that you created has a Java file `App.java` that looks like this:

```Java
/*
 * This source file was generated by the Gradle 'init' task
 */
package org.example;

public class App {
    public String getGreeting() {
        return "Hello World!";
	}

    public static void main(String[] args) {
        System.out.println(new App().getGreeting());
    }
}
```

This shows that your `App` class is in the package `org.example`. That package is also not available automatically in `jshell`. But we cannot even import it!

```Java
jshell> import org.example.App
|  Error:
|  package org.example does not exist
|  import org.example.App;
|         ^-------------^

jshell> 
```

The reason is that you have to tell `jshell` where to look for the classes in your application. You can do that when starting `jshell` using the `--class-path` option, just like we did for the `java` program in the previous lesson.

We can also augment the classpath inside the `jshell` like this:

```Java
jshell> /env --class-path app/build/classes/java/main
|  Setting new options and restoring state.

jshell>
```

Now we can import the `App` class.

```Java
jshell> import org.example.App

jshell>
```

And now we can create an object from your class!

```Java
jshell> var app = new App()
app ==> org.example.App@4b952a2d

jshell>
```

And we can inspect its class, and even call the `getGreeting` method.

```Java
jshell> app.getClass()
$15 ==> class org.example.App

jshell> app.getGreeting()
$16 ==> "Hello World!"

jshell>
```
