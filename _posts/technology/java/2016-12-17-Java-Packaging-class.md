---
layout: post
title:  "Java包装类解读"
date:   2016-11-17 12:31:01 +0800
categories: Java基础
tag: Java
---

* content
{:toc}



1.基本类型和包装类对应关系
=======================================

除了int和char例外，其他均首字母大写
基本数据类型 | 包装类
---|---
byte|Byte
short|Short
int|Integer
long|Long
char|Charater
float|Float
double|Double
boolean|Bloolean

2.基本类型和包装类转换
=======================================

**自动装箱和拆箱**
- **自动装箱：** 基本类型-->包装类

  ```Java
    {
        //1.直接把基本变量赋值给Integer对象
        Integer intObj = 5;
        //2.直接把基本变量赋值给Object对象
        Object boolObject = true;
    }

  ```
  - **自动拆箱：** 基本类型<--包装类
  ```Java
    {
        //1.直接把Integer赋值给int
        int it = intObj;

        //2.先把Object强制转换成Boolean，再赋值给boolean
        if (boolObject instanceof Boolean){
            boolean b = (Boolean)boolObject;
        }
    }

  ```

  **注意:** 自动拆箱和装箱只能在对应类型之间，例如int和Integer。

3.包装类和字符串直接转换
=======================================

字符串转化成基本类型

```Java
    {
        String intStr = "111";

        //1.利用包装类提供的Xxx(String x)构造器
        int int1 = new Integer(intStr);

        //2.利用包装类提供的parseXxx(String x)静态方法
        int int2 = Integer.parseInt(intStr);
    }
```
 **注意:** Character包装类无此parseXxx(String x)静态方法

基本类型转化成字符串
```Java
    {
        int it1 = 111;

        //1.通过String.valueOf(xxx)转换
        String str1 = String.valueOf(int1);

        //2.将基本变量和""进行连接运算
        String str2 = it1 + "";
    }
```

4.包装类，基本变量相互比较
=======================================

包装类和基本变量比较，可以直接进行数值比较
```Java
    {
        Integer a = new Integer(100);
        //输出true
        System.out.println("100的包装类实力是否大于50:"+(a > 50));
    }
```
包装类直接相互比较

  - 包装类实例实际是引用类型，只有两个包装类引用同一个对象时才会返回true
    ```Java
    {
        //输出false
        System.out.println("比较100的包装类是否相等:"+
        (new Integer(100) == new Integer(100)));
    }
    ```
 **特殊情况(部分面试会问）：应用常量池的场景** 自动装箱可以把一些基本类型赋值给一个包装类实例，但会有以下特殊情况~
```Java
    public static void main(String[] args)
	{
        //Java在编译的时候会直接将代码封装成Intege从而使用常量池中的对象
		Integer ina = 2;
		Integer inb = 2;
		System.out.println("两个2自动装箱后是否相等"+ (ina == inb)); // 输出true

		//超出缓存数组范围，无法使用常量池
		Integer biga = 128;
		Integer bigb = 128;
		System.out.println("两个2自动装箱后是否相等"+ (biga == bigb)); // 输出false
	}
```
**直接看源码分析：**
    系统会把一个-128~127之间的数值自动装箱成Integer实例，并放入cache数组缓存起来，如果以后还有这个范围的整数自动装箱，实际上是指向同一个数组元素，但不在这个范围内的，系统会重新创建一个Integer实例。

```Java
    private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    int i = parseInt(integerCacheHighPropValue);
                    i = Math.max(i, 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);

            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
        }

        private IntegerCache() {}
    }

    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```

**同上，也使用cache的如下面列表所示：**


包装类 | 缓存数组范围
---|---
Byte| static final Byte cache[] = new Byte[-(-128) + 127 + 1];
Short | static final Short cache[] = new Short[-(-128) + 127 + 1]
Long|  static final Long cache[] = new Long[-(-128) + 127 + 1];
Character|static final Character cache[] = new Character[127 + 1];
Float|无
Double|无
Boolean|无

创建新的对象
```Java
   {
        i1 = 40;
        //创建新的对象
        Integer i2 = new Integer(40);
        System.out.println(i1==i2);//输出false
   }
```
**综合举例**
```Java
   {
       public static void main(String[] args) {
        int i0 = 127;
        Integer i1 = 127;
        Integer i2 = 127;
        Integer i3 = 0;
        Integer i4 = new Integer(127);
        Integer i5 = new Integer(127);
        Integer i6 = new Integer(0);

        System.out.println("i0=i1   "  +(i0==i1));//true
        System.out.println("i0=i4   "  +(i0==i4));//true

        System.out.println("i1=i2   " + (i1 == i2));//true
        System.out.println("i1=i2+i3   " + (i1 == i2 + i3));//true
        System.out.println("i1=i4   " + (i1 == i4));//false

        System.out.println("i4=i5   " + (i4 == i5));//false
        System.out.println("i4=i5+i6   " + (i4 == i5 + i6));//true
        System.out.println("127=i5+i6   " + (127 == i5 + i6));//true
        System.out.println("---------------------------");

        int i20 = 128;
        Integer i21 = 128;
        Integer i22 = 128;
        Integer i23 = 0;
        Integer i24 = new Integer(128);
        Integer i25 = new Integer(128);
        Integer i26 = new Integer(0);

        System.out.println("i20=i21   "  +(i20==i21));//true
        System.out.println("i20=i24   "  +(i20==i24));//true

        System.out.println("i21=i22   " + (i21 == i22));//!!!!!!false
        System.out.println("i21=i22+i23   " + (i21 == i22 + i23));//true
        System.out.println("i21=i24   " + (i21 == i24));//false

        System.out.println("i24=i25   " + (i24 == i25));//false
        System.out.println("i24=i25+i26   " + (i24 == i25 + i26));//true
        System.out.println("128=i25+i26   " + (128 == i25 + i26));//true
    }
   }
```
   **解释：**
1. int和其他Integer进行==比较是会自动拆箱为int类型。
2. 语句i4 == i5 +i6，因为+这个操作符不适用于Integer对象，是进行数值运算，要首先分别对i5和i6进行自动拆箱操作，进行数值相加，即i4 == 127。然后Integer对象无法与数值进行直接比较，所以i4自动拆箱转为int值127，最终这条语句转为127 == 127进行数值比较。
