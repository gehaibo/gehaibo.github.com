---
layout: post
title:  "String常量池的深入理解"
date:   2017-01-11 13:31:01 +0800
categories: Java基础
tag: Java
---


* content
{:toc}



1.常量池概念
=======================================

1.1 常量池
----------------------------------

在java用于保存在编译期已确定的，已编译的class文件中的一份数据。它包括了关于类，方法，接口等中的常量，也包括字符串常量，如String s = "java"这种申明方式；当然也可扩充，执行器产生的常量也会放入常量池，故认为常量池是JVM的一块特殊的内存空间
[百度百科](http://baike.baidu.com/link?url=lX6ZWhY7GLm06XdFfUzKbVxgIf-2sLgOo8t9-OCapARUC2As3V7yOEJQJOjZKag-kMkyBReNf--Gl0jxHKQwH-AmPk7Ce9my8U_ELJWMPfHVU3-aVCoCk5l79KAJLwpq)

---

2.String类和常量池
=======================================

2.1 String的"==" 和 "equals()"
----------------------------------

- **==** 由于String是引用类型,只有指向同一个对象时，才会==判断为true。
- **equals()** String已经重写了Object的equals()方法，只要两个字符串所包含的字符序列相同就会返回true。
### 2.2 String对象创建方式
- 产生一个字符串对象
```Java
{
    //编译时即可计算出字符串值，JVM在常量池创建字符串对象，管理"abc"
    String s1 = "abc";
}
```
- 产生两个字符串对象
```Java
{
    //JVM会先使用常量池创建管理字符串对象，再调用String类构造器来创建一个新的String对象。
    String s2 = new String("abc");

    System.out.println(s1 == s2);//输出false
    System.out.println(s1.equals(s2));//输出true
}
```
2.3 连接表达式 "+"
----------------------------------

- 字符串**直接量**使String对象之间使用"+"连接产生的新对象才会被加入字符串池中。(区分关键是编译时是否能确认出直接量的值)
```Java
{
    String s1 = "a";
    String s2 = "bc";
    String s3 = "abc";

    //编译时可以确定，可以引用常量池中字符串
    String s4 = "a"+"b"+"c";
    //编译时无法确定，不能引用常量池中字符串
    String s5 = s1+s2;
    String s6 = new String("abc");

    System.out.println(s3 == s4);//true
    System.out.println(s3 == s5);//false
    System.out.println(s3 == s6);//false
}
```
2.4 intern()
----------------------------------

- java.lang.String.intern() 方法返回一个字符串对象的规范表示。当调用 intern 方法时，如果池已经包含一个等于此 String 对象的字符串（该对象由 equals(Object) 方法确定），则返回池中的字符串。否则，将此 String 对象添加到池中，并且返回此 String 对象的引用。
- 它遵循对于任何两个字符串 s 和 t，当且仅当 s.equals(t) 为 true 时，s.intern() == t.intern() 才为 true。

```java
{
    String str1 = "a";
    String str2 = "b";
    String str3 = "ab";

    String str4 = str1 + str2;//常量池中已经存在，直接取出
    String str5 = "a" + "b";

    System.out.println(str3 == str4); // false
    System.out.println(str3 == str4.intern()); // true
    System.out.println(str3 == str5);// true
}
```
