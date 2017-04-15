[TOC]

# 基本概念理解

NIO 类似于流，但又有不同：
- 既可以从通道中读取数据，又可以写数据到通道。但流的读写是单向的。
- 通道可以异步的读写
- 通道中的数据总是要先读到一个 Buffer ，或者总是要从一个 Buffer 中写入。
![NIO读写](https://raw.githubusercontent.com/Arnold4869/note/master/images/nio01.png)

# Channel 的基本实现
- FileChannel:从文件中读写数据
- DatagramChannel：能通过 UDP 读写网络中的数据
- SocketChannel：能通过 TCP 读写网络中的数据
- ServerSocketChannel：可以监听新进来的 TCP 连接，像 Web 服务器那样，对每一个新进来的连接都会创建一个SocketChannel

# 使用 Buffer 的基本用法
- 写入数据到 Buffer
- 调用 `flip()` 方法
- 从 Buffer 中读取数据
- 调用 `clear()` 方法或者 `compact()` 方法

## Buffer 类型

- ByteBuffer
- MappedByteBuffer
- CharBuffer
- DoubleBuffer
- FloatBuffer
- IntBuffer
- LongBuffer
- ShortBuffer

## capacity/position/limit

![NIO_capacity/position/limit关系](https://raw.githubusercontent.com/Arnold4869/note/master/images/NIO_buffer01.png)


## Buffer 常用方法

### Buffer 的分配： `allocate()`

```java
ByteBuffer buf = ByteBuffer.allocate(48);
```

### 向 Buffer 中写数据

写数据到 Buffer 中有两种方式：
- 从 Channel 写到 Buffer.
- 通过 Buffer 的 `put()` 方法写到 Buffer

**从 Channel 写到 Buffer 的例子**

```java
int byteRead = inChannel.read(buf);//read into buffer
```

**通过 `put()` 方法写 Buffer 的例子**

```java
buf.put(127);
```

### 切换模式 `flip()`

`flip()` 方法将 Buffer 从**写模式切换到读模式**
**调用 `flip()` 方法会将 position 设置为 0 ，并将 limit 设置成之前 position 的值
换句话说：position 用于标记读的位置，limit 表示之前写进了多少个 byte/char等（即现在能读取多少 byte/char ）**

### `rewind()`
Buffer.rewind() 将 position 设回 0，所以可以重读 Buffer 中的所有数据。limit 保持不变，仍然表示能从 Buffer 中读取多少个元素(byte/char)

### `clear()` 与 `compact()`
一旦读完 Buffer 中的数据，需要让 Buffer 准备好再次被写入。这时可以通过 `clear()` 或 `compact()` 方法来完成
如果调用的是 `clear()` 方法，position 将被设回 0，limit 被设置成 capacity 的值。换句话说，Buffer 被清空了。Buffer 中的数据并未清除，只是这些标记告诉我们可以从哪里开始往 Buffer 里写数据。
如果 Buffer 中有一些未读的数据，调用 `clear()` 方法，数据将 “被遗忘”，意味着不再有任何标记会告诉你哪些数据被读过，哪些还没有。
如果 Buffer 中仍有未读的数据，且后续还需要这些数据，但是此时想要先先写些数据，那么使用 `compact()` 方法。
`compact()` 方法将所有未读的数据拷贝到 Buffer 起始处。然后将 position 设到最后一个未读元素正后面。limit 属性依然像 `clear()` 方法一样，设置成 capacity。现在 Buffer 准备好写数据了，但是不会覆盖未读的数据。

### `mark()` 与 `reset()`
通过调用 Buffer.mark() 方法，可以标记 Buffer 中的一个特定 position。之后可以通过调用 Buffer.reset() 方法恢复到这个 position.

```java
buffer.mark();
buffer.reset();
```

### `equals()` 与 `compareTo()`
#### equals()

满足以下条件时，表示两个 Buffer 相等
1. 有相同的**类型**
2. Buffer 中剩余的 byte/char 等的个数相等
3. Buffer 中所有剩余的 byte/char 等都相同

**`equals()` 知识比较 Buffer 的一部分;实际它只比较 Buffer 中剩余的元素**
#### compareTo()
compareTo() 方法比较两个 Buffer 的剩余元素 (byte、char 等)， 如果满足下列条件，则认为一个 Buffer“小于” 另一个 Buffer：
1. 第一个不想等的元素小于另一个 Buffer 中对应的元素
2. 所有元素都相等，但第一个 Buffer 比另一个先耗尽（第一个 Buffer 元素个数比另一个少）

