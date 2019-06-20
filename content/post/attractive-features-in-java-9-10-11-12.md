---
title: "Attractive Features in Java 9, 10, 11 and 12"
date: 2019-04-12
draft: true
tags:
 - "Java"
categories:
 - "programming"
---



Last month Oracle released Java 12 on time, just six months after Java 11, another LTS version after Java 8. Though we are still using Java 8 in production and unlikely to upgrade to 11 in the near future, it's advisable to learn the new features to keep pace with the Java world. In my opinion, from a programmer's view, the following are the most attractive features. Since some features evolved during the four versions, only the latest usage will be covered.

# Language

In this section I'll focus on the modular system and syntax improvements, or syntax sugars.

## module system (since Java 9)

The Java Platform Module System, known as Project Jigsaw, has been delayed for years, and finally becomes available in Java 9. 


## type inference: var (since Java 10)

Local variables could be declared with *var* keyword if they're initialized and their type would be inferred automatically.

Here're some examples:

```java
public void test() {
    var i = 1; // int
    var l = 1L; // long
    var f = 0.1f; // float
    var d = 0.1; // double
    var s = "hello"; // String
    
    List<String> list = new ArrayList<>(); // old style
    var list2 = new ArrayList<String>(); // var style, type in the collection should be specified explicitly
    
    Iterator<List<String>> iter = list.iterator(); // verbose old style
    var iter2 = list.iterator(); // much more simpler
        
    // var in lambda since Java 11
    list.forEach((var s) -> System.out.println(s));
}
```

Though it's a syntax sugar, it could reduce the typing, especially for those verbose collection types.  While C++ has introduced `auto` for a long time (since C++ 11), Java has a much slower pace.



## switch expression (since Java 12)




# API



# JVM

## garbage collection



# Tools

# Summary



# Reference


* [Mastering Java 11 - Second Edition](https://www.oreilly.com/library/view/mastering-java-11/9781789137613/)
* [Java SE New Features: Covers Versions 9, 10, 11, and 12](https://www.oreilly.com/library/view/java-se-new/9781789610062/)
* [Upgrading from Java 8 to Java 12](https://www.infoq.com/articles/upgrading-java-8-to-12)

