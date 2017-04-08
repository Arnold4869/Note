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

## 3. java 的 clone
java 中有两种方式创建一个新的**对象**
1. 使用 `new` 操作符**创建**一个对象
2. 使用 `clone` 操作符**复制**一个对象

区别：
那么这两种方式有什么相同和不同呢？
`new` 操作符的本意是**分配内存**。程序执行到 `new` 操作符时， 首先去看 `new` 操作符后面的类型，因为知道了类型，才能知道要分配多大的内存空间。分配完内存之后，再调用**构造函数**，填充对象的各个域，这一步叫做对象的初始化，构造方法返回后，一个对象创建完毕，可以把他的引用（地址）发布到外部，在外部就可以使用这个引用操纵这个对象。

而 `clone` 在第一步是和 `new` 相似的，都是分配内存，调用 `clone` 方法时，分配的内存和源对象（即调用 `clone` 方法的对象）相同，然后再使用原对象中对应的各个域，填充新对象的域， 填充完成之后，`clone`方法返回，一个新的相同的对象被创建，同样可以把这个新对象的引用发布到外部。

### 浅拷贝 or 深拷贝
![浅拷贝和深拷贝](http://img.blog.csdn.net/20140116224712140?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemhhbmdqZ19ibG9n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

对于浅拷贝，对象内部引用型的指向和原型对象内部引用型指向**一致**
而深拷贝，则是内部引用型的指向和原有的不一致，也经过拷贝处理

**单纯的 `clone` 实现的是浅拷贝，深拷贝需要特殊处理**

#### 深拷贝实现

现在为了要在 `clone` 对象时进行深拷贝，那么就要 `Clonable` 接口，覆盖并实现 `clone` 方法，除了调用父类中的 `clone` 方法得到新的对象，还要将该类中的引用变量也 `clone` 出来。对应的，内部对象也需要实现 `Clonable` 接口，覆写 `clone` 方法