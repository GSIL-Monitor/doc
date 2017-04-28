#### JAVA 序列化

* 以上是wiki对序列化(Serialization)的定义。核心点是『将特定结构的数据转化为另一种能被存储和还原的格式』。有一位伟人说过，『序列化后的数据格式可以使二进制的，也可以是其他格式的（比如XML、JSON、YAML等），但归根结底还是二进制的。』，因为即使是XML、JSON等格式，最终还是会通过特定编码转化为机器能识别的字节，进行存储或网络传输。
* 本文着重介绍Java的二进制序列化方式，通过实验对比了几种常见的Java序列化框架，最后对身边几个应用场景下的序列化方案选型做简单的分析。

#### 感受Java序列化
* 下面是我简单梳理的在做Java的序列化方案选型时可以考虑的点：
   * 基础能力：提供序列化、反序列化的能力。
   * 正确性：对Java继承、集合、泛型以及对象引用关系（特殊的比如循环引用）的支持是否完备，这些都可能带来不可预知的bug。
   * 开发成本：有的序列化方式可以无缝接入业务系统，有的则需要额外的配置，以具体场景能容忍的开发成本为准。
   * 时间开销：序列化过程中，JVM的GC机制、类加载反射机制会影响时间开销，另外由于IO的成本一般大于内存、CPU的计算的成本，时间开销也往往强依赖于序列化的空间开销，序列化文件越大相应的时间开销也越大。
   * 空间开销：包括序列化过程中动态的内存空间占用量，以及序列化后产出的文件大小。
   * 安全性：因为序列化常涉及到存储和传输，安全性需要考虑。但同时由于序列化的过程常发生在内部网络，外输出的数据则有应用层的协议保障安全，故一般情况下，序列化对安全性要求不高。
   
   * 兼容性：前向后向兼容：从业务演化的角度，可能出现序列化前后数据模式的改变。比如RPC过程中调用端和被调用端的Java类字段的改变。这就要求序列化有前向、后向兼容性，以此保证业务流程在业务演化过程中的稳定性。
   * 跨语言兼容：大多数情况下同一架构体系内的应用采用同样的语言框架，不用考虑跨语言兼容。当某些互相耦合的模块之间不可避免的需要不同语言的应用来支持时，就需要序列化协议具有跨语言的能力。本文偷懒暂不讨论跨语言兼容的问题=)。

#### Java二进制序列化的套路

序列化本质上就是一种编解码方式。这里只考虑将Java对象编码为二进制的套路。
普通的Java对象的数据通过字段的方式组织，类似于KV的形式，序列化后的二进制数组需要能表达不同字段的拆分，即数据单元的拆分和映射。广义而言，有两种方式实现数据单元的拆分、映射：
动态携带meta信息：即类似通过消息头+消息体的方式，将类信息携带到序列化后的二进制数据中。
Java原生序列化：将类信息用字符串的形式全量的写入二进制流，来粗暴的实现序列化。这是比较极端和稳妥的方式，效率低，但不会出错。
Hessian、Kryo等：将meta信息写入二进制流，但并不是每个对象都保存了meta信息.特殊的编码方式提高了序列化效率。同事kryo也提供了在序列化前类注册的方式，者其实是用反射的机制来实现类似『静态meta信息文件』，提升效率。
静态的meta信息文件：通过定义额外的静态meta信息文件，来指导数据的拆分、映射。
protobuf、thrift等：借助中间文件，提升传输效率和兼容性。其中IDL(Interface Define Language)文件存储了字段的类型、顺序等信息，在二进制流中则包含了字段id，类型、长度信息，从而减少二进制流的大小，提升序列化速度。
其中前者空间开销偏大。而后者相当于缓存了meta信息，执行上更高效，当然这会引入开发流程中的额外工作量。
【这里再岔开，讲的多点】
难点问题：
泛型
对象引用、循环引用
继承
常见Java序列化方案比较

1. Java原生序列化

实现了java.io.Serializable接口的类，可以被执行串行化操作。通过serialVersionUID来验证版本的一致性，否则抛出异常。
优点
稳定无bug
开发接入成本低。
对Java继承体系提供完整的支持。
支持自定义序列化的方式，可实现自定义的加密等措施（重写readObject，writeObject方法）
提供了安全性的支持（比如使用javax.crypto.SealedObject或java.security.SignedObject做装饰）
缺点
慢
空间大
存储了所有元信息到每个对象的二进制流。
所有字段信息用字符串写入二进制流，空间很大。
无法跨语言，java强绑定
使用中的坑
如果内部的字段的类没有实现java.io.Serializable，也会报错。
子类实现了java.io.Serializable接口，父类没有，则子类中属性能正确序列化，父类中的属性无法序列化，数据丢失。
序列化的示例代码如下，序列化对代码几乎无侵入，接入成本低。
        ByteArrayOutputStream baos = outputStream(data);
        ObjectOutputStream oos = new ObjectOutputStream(baos);
        oos.writeObject(data);
        return baos.toByteArray();
另外还有一种Java原生的externalization的方式，通过自己实现writeExternal(ObjectOutput)和readExternal(ObjectInput)来完成对象的序列化。这种全权交与开发者来处理序列化细节的方式在实践中很少见。
2. Hessian

hessian原本是一个RPC框架，基于http传输，通过自定义的序列化协议将数据转换成二进制流，支持跨语言兼容性。由于其中序列化协议在稳定性和效率的折中上表现优异，切接入凌晨本，被广泛在互联网应用中使用。这里仅讨论hessian串行化协议(中文译版)。
特点
时间、空间开销优于Java原生的方式，逊于kryo、protobuff等其他方式。
对继承支持不足。由于其跨语言的特请，没有在继承关系上有充足考虑，在继承体系上有很多bug。
一些坑
子类和父类同名字段会被覆盖：参考
对继承的支持不友好。比如继承自HashMap的类的添加字段无法被序列化：参考
不支持某些集合类：参考
枚举值，反序列化通过valueof()方法，如果服务端增加枚举项后，可能出现不兼容。
使用Hessian序列化的类，同样需要实现java.io.Serializable接口。调用代码类似于Java原生序列化：
        ByteArrayOutputStream out = outputStream(data);
        Hessian2StreamingOutput hout = new Hessian2StreamingOutput(out);
        hout.writeObject(data);
        return out.toByteArray();
3.kryo

kryo实现思路和hessian类似，性能优于hessian。
kryo二进制流中没有类的字段描述信息，这减小了二进制流的大小。字段描述信息通过向kryo注册类来主动加载，或者在第一次被序列化、反序列化时被动加载。
通过变长编码来记录int、long型，节省空间。
缺点有：
前后向兼容差，不支持改动字段。因为二进制流中没有字段描述信息。
跨语言不支持
开发成本：必须有无参构造函数（hessian不需要），但不需要实现Serializable接口（hessian一定要实现）
它在Twitter、Apache Hive、Akka等产品中被广泛使用。
类似的序列化框架还有fast-serialization，速度快、体系小，但缺点同样明显：不支持前后向兼容。
4. proto-buffers

protocol-buffers是Google开源的跨语言编码协议。Google内部的几乎所有RPC都使用了该协议。跟hessian类似，作为致力于跨语言的协议，pb并没有花精力在支持Java的继承体系上。
pb的开发成本很高。需要定义.proto文件，并用工具生成对应的辅助类。该类特有一些序列化的辅助方法，所有要序列化的对象，都需要先转化为辅助类的对象，这让序列化代码跟业务代码大量耦合，是侵入性较强的一种方式。由于代码量较大，这里不展开相应的调用代码。
优点：
跨语言兼容
前后向兼容性好：字段匹配依赖于顺序，添加字段注意往后面加，不要删除字段。
支持基本类型的自动转换，比如int转long
性能优势：编码后size小，编解码速度快
缺点：
不支持继承。
需要自己写proto文件，定义schema，并生成对应的源码文件。开发繁琐，侵入了业务代码。代码示例参见：protobuf的Java代码示例
值得一提的是，在协议设计上，pb做了很多工作，让它拥有非常高的效率。包括并不限于以下点。有兴趣可以参照文档了解：protocol-buffers编码规则
按照.proto文件定义的字段顺序，存储字段值，避免了重复记录字段名。
紧凑编码：varint，用MSB(高bit位)确定总体长度。
zigzag编码，区别于补码实现的正负数，数值小时，负数所需字节少。
5. proto stuff：

proto stuff是一个集成了多重协议的序列化框架。像它的名字一样，它也提供了对protobuf协议的支持。这里介绍protostuff的runtime功能，兼具了不用手写IDL文件的省事儿，又能获得较好的性能优势。
优点
兼容protobuf
向前向后兼容
速度快、编码后size小
支持基本类型转换
支持多种序列化格式，包括protobuf，protostuff，json，xml，yaml等……
支持运行时生成schema，不用手写，开发接入成本低。
注意点：由于protostuff默认基于JVM中类的字节码中字段的顺序做字段匹配，由于JVM specification没有明确定义字节码层面对字段顺序的要求，不同JVM可能不一样。要保证足够的兼容性，建议使用@Tag标签定义字段顺序。
下面是protostuff-runtime的示例代码：
        final Schema<data.media.MediaContent> schema =         RuntimeSchema.getSchema(data.media.MediaContent.class);
        final LinkedBuffer buffer = LinkedBuffer.allocate(BUFFER_SIZE);

        public data.media.MediaContent deserialize(byte[] array) throws Exception
        {
            data.media.MediaContent mc = new data.media.MediaContent();
            ProtostuffIOUtil.mergeFrom(array, mc, schema);
            return mc;
        }

        public byte[] serialize(data.media.MediaContent content) throws Exception
        {
            try
            {
                return ProtostuffIOUtil.toByteArray(content, schema, buffer);
            }
            finally
            {
                buffer.clear();
            }
        }
6. JSON类等

JSON的格式本身有强语义，可读。把它作为中间结果，便于问题排查。其中较快的库有alibaba的fastJson。把它用作序列化框架，其特点有：
中间结果可读，易于排查
跨平台、跨语言支持
速度快、但体积大
由于多了层JSON的解析，引入了更多稳定应隐患
性能测试

实验针对以上常见序列化方式的性能进行了比较。-- powerd by jvm-serializers in github。
实验环境：
Mac OS X 10.10.4
JVM: jdk 1.8.0_74
CPU: 1.6 GHz Intel Core i5
Cores (incl HT):4
无共享引用的测试

无循环引用。 一个对象如果被引用两次则会序列化两次
没有手工优化
schema预先已知
Ser Time+Deser Time (ns)
 
Size, Compressed size [light] in bytes
 
                                   create     ser   deser   total   size  +dfl
protobuf                              677    1302    1032    2334    242   152
fst-flat-pre                          102    1001    1383    2383    254   168
kryo-flat-pre                         110    1048    1436    2484    216   136
protostuff                            144    1280    2170    3451    242   153
thrift-compact                        132    2315    1408    3723    243   152
thrift                                143    2536    1343    3879    352   201
java-built-in                         134    9852   63422   73274    892   520
全对象图测试

支持全部的对象图读写，对象图可能包含循环引用
无预先处理，没有预先的类生成、注册.。所有都运行时产生，比如使用反射
注意通常不会跨编程语言，然而JSON/XML格式由于其特殊性可以跨语言
Ser Time+Deser Time (ns)
 
Size, Compressed size [light] in bytes
 
                                   create     ser   deser   total   size  +dfl
protostuff-graph                      129    1158    1358    2516    242   153
protostuff-graph-runtime               98    1271    1364    2635    244   154
fst                                    95    2680    2642    5322    319   208
kryo-serializer                        98    2865    2803    5667    290   192
hessian                               113    4964    7481   12445    504   319
java-built-in-serializer              104    9509   55342   64851    892   520
一些序列化应用场景

1.RPC框架HSF

HSF旨在为阿里巴巴的应用提供一个透明的分布式服务框架。
为了让业务开发无感知，序列化时不可以引入IDL文件。作为性能和可靠性的折中，HSF默认使用hessian2做序列化，同时HSF团队也在不断优化hessian协议，比如原本hessian对类的meta信息的缓存是在HSF请求级别的，最新的改动会将缓存放到传输层，即TCP连接里，以进一步减少传输字节流的大小（参考）。
2.TAIR

Tair是分布式KV存储系统。他要求序列化的对象都实现Serializable接口。序列化时，TAIR针对对象进行了基于类型的路由，核心代码如下：
//……
        //特殊类型，注入基本类型、装箱类型、Stirng等，特殊处理
        if (object instanceof String) {
            b = TranscoderUtil.encodeString((String) object, charset);
            flag = TairConstant.TAIR_STYPE_STRING;
//……
       //如果是byte[]，不需要序列化直接传入
        } else if (object instanceof byte[]) {
            b = (byte[]) object;
            flag = TairConstant.TAIR_STYPE_BYTEARRAY;
//……
       //如果是else的情况，则用Java原生序列化方式
        } else {
            b = TranscoderUtil.serialize(object);
            flag = TairConstant.TAIR_STYPE_SERIALIZE;
        }
其核心思想还是保证数据存取的稳定性，毕竟KV存储平台里，存储的对象都是五花八门的，无疑Java原生的序列化方式是最稳定的。如果应用实在有更高的性能要求，可以对自己的对象自行序列化、反序列化来做优化。
有些小坑值得注意，比如存储到tair的对象中，如果有字段没有实现Serializable接口，进行存取操作时返回错误码报错。
3.metaQ

MetaQ比Tair更直接，传送的就是byte数组。
4.xspac-account的全量本地缓存数据传输

xspace-account是xspace平台中的重要一环。作为微服务体系中的账号系统，维护着在线云客服所有的客服信息。
初期的架构方案是，业务应用拉取服务器的人员信息大对象的序列化文件，加载到本地JVM使用。每次更新后服务器会发送消息，通知应用全量重新拉取文件。
在这个场景下，用户信息使用Java原生序列化得到的文件大小约100M，不利于网络传输。技术上的需求是：
尽可能减少文件大小，减轻网络传输的负担
业务字段调整时有发生，保证前后向兼容性
序列化没有强实时性要求，可以向文件大小倾斜，适当放宽对序列化、反序列化时间的限制
其中尝试过FST序列化，速度和文件大小表现优异，但引起过fullGC，以及前后向兼容性差带来的问题。综合考虑，一个更平衡的方案是protostuff-runtime + gzip压缩的方式。对于protobuf的序列化文件，gzip还能压缩50%的空间，很是给力。
末了

有时候，未必需要我们自己来序列化，就像metaq做的那样；有时候，未必一定要用序列化来压缩体积，比如xspace-account的gzip做的那样；有时候，同一个序列化框架用其他对象跑的测试数据，可能跟我们的业务场景下的数据的效果不一样，都得依情况而论。
没有最好的技术，只有最适合当前场景的技术。
其他参考

《Kryo为什么比Hessian快》
5 Things You Didn't Know About - Java Object Serialization>
Serialization and Performance in Java