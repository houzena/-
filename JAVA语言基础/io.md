按照流是否直接与特定的地方（如磁盘、内存、设备等）相连，分为节点流和处理流两类。

- 节点流：可以从或向一个特定的地方（节点）读写数据。如FileReader.
- 处理流：是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写。如BufferedReader.处理流的构造方法总是要带一个其他的流对象做参数。一个流对象经过其他流的多次包装，称为流的链接。

**JAVA常用的节点流：**

- 文 件 FileInputStream FileOutputStrean FileReader FileWriter 文件进行处理的节点流。
- 字符串 StringReader StringWriter 对字符串进行处理的节点流。
- 数 组 ByteArrayInputStream ByteArrayOutputStreamCharArrayReader CharArrayWriter 对数组进行处理的节点流（对应的不再是文件，而是内存中的一个数组）。
- 管 道 PipedInputStream PipedOutputStream PipedReaderPipedWriter对管道进行处理的节点流。

**常用处理流（关闭处理流使用关闭里面的节点流）**

- 缓冲流：BufferedInputStrean BufferedOutputStream BufferedReader BufferedWriter  增加缓冲功能，避免频繁读写硬盘。

- 转换流：InputStreamReader OutputStreamReader 实现字节流和字符流之间的转换。
- 数据流 DataInputStream DataOutputStream  等-提供将基础数据类型写入到文件中，或者读取出来.

### 流的关闭顺序

1. 一般情况下是：先打开的后关闭，后打开的先关闭
2. 另一种情况：看依赖关系，如果流a依赖流b，应该先关闭流a，再关闭流b。例如，处理流a依赖节点流b，应该先关闭处理流a，再关闭节点流b
3. 可以只关闭处理流，不用关闭节点流。处理流关闭的时候，会调用其处理的节点流的关闭方法。