# Buffer

node中处理二进制的模块，因为是常用模块，无需单独引入

Buffer提供了几个方法

* Buffer.from() 转换成buffer
* * Buffer.from(array)
* * Buffer.from(buffer)
* * Buffer.from(array.buffer,[byteOffset])
* * Buffer.from(string,[encoding])

* Buffer.alloc(size,[fill,[encoding]])  创建size大小的buffer，fill是填充值，默认填充0

* Buffer.allocUnsafe(size)  速度比alloc快，因为没有填充，可能读取以前的数据，导致不安全，需要自己手动去fill

* Buffer.byteLength(buffer) 查看字节长度

* Buffer.concat(buffer,[totalLength])

* Buffer.isBuffer(obj)