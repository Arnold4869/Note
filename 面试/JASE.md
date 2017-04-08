[TOC]
## 1. int 和 Integer 区别
1. 无论如何，`Integer`与`new Integer`不会相等。不会经历拆箱过程，i3的引用指向堆，而i4指向专门存放他的内存（常量池），他们的内存地址不一样，所以为`false`
2. 两个都是非**new**出来的`Integer`，如果数在`-128`到`127`之间，则是`true`,否则为`false`。原因是java在编译Integer i2 = 128的时候,被翻译成-> Integer i2 = Integer.valueOf(128);而valueOf()函数会对-128到127之间的数进行缓存
3. 两个都是**new**出来的,都为`false`
4. `int`和`integer`**(无论new否)**比，都为`true`，因为会把`Integer`自动拆箱为`int`再去比

## 2. String / StringBuffer / StringBuilder
`String`：不可变、不可继承
`StringBuffer`：可变，线程安全
`StringBuilder`:可变，线程不安全

在大部分情况下，字符串拼接速度：
`StringBuilder` > `StringBuffer` > `String`
具体参见[String/StringBuffer/StringBuilder](http://blog.csdn.net/rmn190/article/details/1492013)