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
