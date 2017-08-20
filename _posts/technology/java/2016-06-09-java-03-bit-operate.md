---
layout: post
title:  Java中的位操作(移位，补位，位与，位或，位异或，位非)
date:   2016-06-09 10:43:00 +0800
categories: Java基础
tag: Java
---

* content
{:toc}


> int占4个byte,每个byte占8个bit，共32个bit，第一位是符号位

> 计算机内部十进制数字是按照补码形式存在并且参与计算的，正数的原码，反码，补码都是本身，负数的反码是原码符号位不变，其他按位取反，补码为反码加一


{% highlight java %}
public static void main(String[] args) {

		/**
		 * 1.左移(int占4个byte,每个byte占8个bit，共32个bit，第一位是符号位)
		 * 0000 0000 0000 0000 0000 0000 0000 0111 (7的补码)左移3位,低位补0
		 * 0000 0000 0000 0000 0000 0000 0011 1000 换算成10进制为(56)
		 */
		System.out.println("左移3位:" + (7 << 3));

		/**
		 * 2.右移
		 * 1000 0000 0000 0000 0000 0000 0011 1000 (-56)
		 * 1000 0000 0000 0000 0000 0000 0011 0111 (-56)的反码
		 * 1111 1111 1111 1111 1111 1111 1100 1000 (-56)的反码加一获得补码
		 * 1111 1111 1111 1111 1111 1111 1111 1001 (-56)的补码右移三位,高位补1
		 * 1000 0000 0000 0000 0000 0000 0000 1001 结果-1取反获得原码，换算成10进制为(-7)
		 */
		System.out.println("右移3位:" + (-56 >> 3));

		/**
		 * 3.无符号右移-负数(其中左移只有有符号左移，原因是不管有符号，还是无符号，都是在右边补0所以没有区别)
		 * 1000 0000 0000 0000 0000 0000 0011 1000 (-56)右移3位,低位补0
		 * 1000 0000 0000 0000 0000 0000 0011 0111 (-56)的反码
		 * 1111 1111 1111 1111 1111 1111 1100 1000 (-56)的反码加一获得补码
		 * 0001 1111 1111 1111 1111 1111 1111 1001 (-56)的补码无符号右移三位,高位补0,获得结果为正数，补码为本身
		 */
		System.out.println("无符号负数右移3位:" + (-56>>> 3));

		/**
		 * 4.无符号右移-正数
		 * 0000 0000 0000 0000 0000 0000 0011 1000 (56)右移3位,低位补0
		 * 0000 0000 0000 0000 0000 0000 0000 0111 换算成10进制为(7)
		 */
		System.out.println("无符号正数右移3位:" + (56>>> 3));

		/**
		 * 5.补位
		 * 对某个数字取固定长度的时候，如果长度不够，高位补0
		 * 如下为取6位
		 */
		int number = 123;
		NumberFormat formatter = NumberFormat.getNumberInstance();
		formatter.setMinimumIntegerDigits(6);
		formatter.setGroupingUsed(false);
		String s = formatter.format(number);
		System.out.println("获取固定长度为6位的数字字符串:" + s);

		/**
		 * 6.位与
		 * 0000 0000 0000 0000 0000 0000 0000 0111 From (7)
		 * 0000 0000 0000 0000 0000 0000 0000 0101 And (5)
		 * 0000 0000 0000 0000 0000 0000 0000 0101 Result (5)
		 */
		System.out.println("位与操作：" + (7 & 5));

		/**
		 * 7.位或
		 * 0000 0000 0000 0000 0000 0000 0000 0111 From (7)
		 * 0000 0000 0000 0000 0000 0000 0000 0101 And (5)
		 * 0000 0000 0000 0000 0000 0000 0000 0111 Result (7)
		 */
		System.out.println("位或操作：" + (7 | 5));

		/**
		 * 8.位异或
		 * 0000 0000 0000 0000 0000 0000 0000 0011 From (3)
		 * 0000 0000 0000 0000 0000 0000 0000 0101 And (5)
		 * 0000 0000 0000 0000 0000 0000 0000 0110 Result (6)
		 */
		System.out.println("位异或操作：" + (3 ^ 5));

		/**
		 * 9.位非
		 * 0000 0000 0000 0000 0000 0000 0000 0101 From (5)(Java中数字按照补码形式存在，所以5个补码是本身)
		 * 1111 1111 1111 1111 1111 1111 1111 1010 取反得到结果的补码(结果的补码是负数)
		 * 1111 1111 1111 1111 1111 1111 1111 1001 补码-1=反码
		 * 1000 0000 0000 0000 0000 0000 0000 0110 结果的源码换算成10进制为（-6）
		 */
		System.out.println("位非：：" + (~5));
	}
{% endhighlight %}

<br />
<br />

参考资料
===========================

补位：[http://piranha.iteye.com/blog/1700952](http://piranha.iteye.com/blog/1700952)

位操作：[http://www.cnblogs.com/jinc/archive/2013/02/26/2934022.html](http://www.cnblogs.com/jinc/archive/2013/02/26/2934022.html)

非运算：[http://tieba.baidu.com/p/3157468882](http://tieba.baidu.com/p/3157468882)

原码, 反码, 补码 详解: [http://www.cnblogs.com/zhangziqiu/archive/2011/03/30/ComputerCode.html](http://www.cnblogs.com/zhangziqiu/archive/2011/03/30/ComputerCode.html)
<br />
<br />