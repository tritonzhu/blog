---
title: "Attractive Features in Java 9, 10 and 11"
date: 2019-01-31
draft: true
tags:
 - "Java"
categories:
 - "programming"
---



A few months ago Oracle released Java 11, another LTS version after Java 8. Though we are still using Java 8 in production, it's advisable to learn the new features for future use. In my opinion, from a programmer's view, the following are the most attractive features. Since some features evolved during the three versions, only the latest usage will be covered.

# Language



## module system (since Java 9)

The Java Platform Module System is a feature delayed for years, and finally available in Java 9. 


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
    var list = new ArrayList<String>(); // type in the collection should be specified
    List<String> list2 = new ArrayList<>(); 
    var iter = list.iterator(); // much more simpler
    Iterator<List<String>> iter2 = list.iterator();
}
```





# API



# JVM

# Tools