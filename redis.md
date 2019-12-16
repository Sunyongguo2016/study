from 敖丙 吊打面试官系列
https://juejin.im/post/5db66ed9e51d452a2f15d833

一。Redis 基础
1. redis数据类型
String、Hash、List、Set、SortedSet ；HyperLogLog、Geo、Pub/Sub
还用过 BloomFilter 布隆过滤器

2.如果大量key同一时间失效，会造成卡顿，严重的话可能造成缓存雪崩。
如果大量数据失效时间十分集中，刚好用户请求又集中在一起，就容易造成缓存雪崩，一般要给失效时间加随机值，分散缓存加载时间节点。

3.redis分布式锁 
先拿setnx来争抢锁，抢到之后，再用expire给锁加一个过期时间防止锁忘记了释放； 如果中途进程中断了，锁就无法释放了；怎么处理？我记得set指令有非常复杂的参数，这个应该是可以同时把setnx和expire合成一条指令来用的

4.假如Redis里面有1亿个key，其中有10w个key是以某个固定的已知的前缀开头的，如何将它们全部找出来
使用keys指令可以扫出指定模式的key列表

5.如果Redis正给业务提供服务，用keys命令有什么问题
这个时候你要回答Redis关键的一个特性：Redis的单线程的。keys指令会导致线程阻塞一段时间，线上服务会停顿，直到指令执行完毕，服务才能恢复；
scan指令可以无阻塞的提取出指定模式的key列表，但是会有一定的重复概率，在客户端做一次去重就可以了，但是整体所花费的时间会比直接用keys指令长

6.redis如何用做异步队列
一般使用list结构作为队列，rpush生产消息，lpop消费消息。当lpop没有消息的时候，要适当sleep一会再重试

7.如果对方追问可不可以不用sleep呢？
list还有个指令叫blpop，在没有消息的时候，它会阻塞住直到消息到来。



8.如果对方接着追问能不能生产一次消费多次呢？

使用pub/sub主题订阅者模式，可以实现 1:N 的消息队列。





9.对方继续追问 pub/su b有什么缺点？
在消费者下线的情况下，生产的消息会丢失，得使用专业的消息队列如RocketMQ等

10.redis如何实现延时队列
使用sortedset，拿时间戳作为score，消息内容作为key调用zadd来生产消息，消费者用zrangebyscore指令获取N秒之前的数据轮询进行处理。

11.Redis是怎么持久化的？服务主从数据怎么交互的？
RDB做镜像全量持久化，AOF做增量持久化。因为RDB会耗费较长时间，不够实时，在停机的时候会导致大量丢失数据，所以需要AOF来配合使用。在redis实例重启时，会使用RDB持久化文件重新构建内存，再使用AOF重放近期的操作指令来实现完整恢复重启之前的状态；
可以理解为rdb是全量数据，aof是命令日志文件，一定时间备份。

12.如果redis 突然断电怎么办
取决于AOF日志sync属性的配置，如果不要求性能，在每条写指令时都sync一下磁盘，就不会丢失数据。但是在高性能的要求下每次都sync是不现实的，一般都使用定时sync，比如1s1次，这个时候最多就会丢失1s的数据


13.Pipeline有什么好处，为什么要用pipeline
可以将多次IO往返的时间缩减为一次，前提是pipeline执行的指令之间没有因果相关性。使用redis-benchmark进行压测的时候可以发现影响redis的QPS峰值的一个重要因素是pipeline批次指令的数目

14.Redis的同步机制了解么
Redis可以使用主从同步，从从同步。第一次同步时，主节点做一次bgsave，并同时将后续修改操作记录到内存buffer，待完成后将RDB文件全量同步到复制节点，复制节点接受完成后将RDB镜像加载到内存。加载完成后，再通知主节点将期间修改的操作记录同步到复制节点进行重放就完成了同步过程。后续的增量数据通过AOF日志同步即可，有点类似数据库的binlog

15.是否使用过Redis集群，集群的高可用怎么保证，集群的原理是什么
Redis Sentinal 着眼于高可用，在master宕机时会自动将slave提升为master，继续提供服务。

Redis Cluster 着眼于扩展性，在单个redis内存不足时，使用Cluster进行分片存储



16.好的 心想这小子这么NB是不是很多Offer在手上，不行我得叫hr给他加钱


总结
在技术面试的时候，不管是Redis还是什么问题，如果你能举出实际的例子，或者是直接说自己开发过程的问题和收获会给面试官的印象分会加很多，回答逻辑性也要强一点，不要东一点西一点，容易把自己都绕晕的


还有一点就是我问你为啥用Redis你不要一上来就直接回答问题了，你可以这样回答：

帅气的面试官您好，首先我们的项目DB遇到了瓶颈，特别是秒杀和热点数据这样的场景DB基本上就扛不住了，那就需要缓存中间件的加入了，目前市面上有的缓存中间件有 Redis 和 Memcached ，他们的优缺点……，综合这些然后再结合我们项目特点，最后我们在技术选型的时候选了谁


如果你这样有条不紊，有理有据的回答了我的问题而且还说出这么多我问题外的知识点，我会觉得你不只是一个会写代码的人，你逻辑清晰，你对技术选型，对中间件对项目都有自己的理解和思考，说白了就是你的offer有戏了



#############################################################
from  敖丙 吊打面试官系列  https://juejin.im/post/5db69365518825645656c0de


# 前言 你在开发或者面试过程中，有没有遇到过 海量数据需要查重，缓存穿透怎么避免等等这样的问题呢？下面这个东西超屌，好好了解下，面试过关斩将，凸显你的不一样


1.Bloom Filter 原理
布隆过滤器的原理是，当一个元素被加入集合时，通过K个散列函数将这个元素映射成一个位数组中的K个点，把它们置为1。检索时，我们只要看看这些点是不是都是1就（大约）知道集合中有没有它了：如果这些点有任何一个0，则被检元素一定不在；如果都是1，则被检元素很可能在。这就是布隆过滤器的基本思想。

Bloom Filter跟单哈希函数Bit-Map不同之处在于：Bloom Filter使用了k个哈希函数，每个字符串跟k个bit对应。从而降低了冲突的概率。



img





2.缓存穿透





每次查询都会直接打到DB

简而言之，言而简之就是我们先把我们数据库的数据都加载到我们的过滤器中，比如数据库的id现在有：1、2、3

那就用id：1 为例子他在上图中经过三次hash之后，把三次原本值0的地方改为1

下次数据进来查询的时候如果id的值是1，那么我就把1拿去三次hash 发现三次hash的值，跟上面的三个位置完全一样，那就能证明过滤器中有1的

反之如果不一样就说明不存在了

那应用的场景在哪里呢？一般我们都会用来防止缓存击穿

简单来说就是你数据库的id都是1开始然后自增的，那我知道你接口是通过id查询的，我就拿负数去查询，这个时候，会发现缓存里面没这个数据，我又去数据库查也没有，一个请求这样，100个，1000个，10000个呢？你的DB基本上就扛不住了，如果在缓存里面加上这个，是不是就不存在了，你判断没这个数据就不去查了，直接return一个数据为空不就好了嘛。

这玩意这么好使那有啥缺点么？有的，我们接着往下看



3.Bloom Filter的缺点
bloom filter之所以能做到在时间和空间上的效率比较高，是因为牺牲了判断的准确率、删除的便利性

存在误判，可能要查到的元素并没有在容器中，但是hash之后得到的k个位置上值都是1。如果bloom filter中存储的是黑名单，那么可以通过建立一个白名单来存储可能会误判的元素。

删除困难。一个放入容器的元素映射到bit数组的k个位置上是1，删除的时候不能简单的直接置为0，可能会影响其他元素的判断。可以采用Counting Bloom Filter

4.Bloom Filter 实现
布隆过滤器有许多实现与优化，Guava中就提供了一种Bloom Filter的实现。

在使用bloom filter时，绕不过的两点是预估数据量n以及期望的误判率fpp，

在实现bloom filter时，绕不过的两点就是hash函数的选取以及bit数组的大小。

对于一个确定的场景，我们预估要存的数据量为n，期望的误判率为fpp，然后需要计算我们需要的Bit数组的大小m，以及hash函数的个数k，并选择hash函数

(1)Bit数组大小选择
  根据预估数据量n以及误判率fpp，bit数组大小的m的计算方式：

img


(2)哈希函数选择
​ 由预估数据量n以及bit数组长度m，可以得到一个hash函数的个数k：

img


​ 哈希函数的选择对性能的影响应该是很大的，一个好的哈希函数要能近似等概率的将字符串映射到各个Bit。选择k个不同的哈希函数比较麻烦，一种简单的方法是选择一个哈希函数，然后送入k个不同的参数。

哈希函数个数k、位数组大小m、加入的字符串数量n的关系可以参考Bloom Filters - the math，Bloom_filter-wikipedia





5 。 要使用BloomFilter，需要引入guava包：

 <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>23.0</version>
 </dependency>    

测试分两步：

1、往过滤器中放一百万个数，然后去验证这一百万个数是否能通过过滤器

2、另外找一万个数，去检验漏网之鱼的数量

/**
 * 测试布隆过滤器(可用于redis缓存穿透)
 * 
 * @author 敖丙
 */
public class TestBloomFilter {

    private static int total = 1000000;
    private static BloomFilter<Integer> bf = BloomFilter.create(Funnels.integerFunnel(), total);
//    private static BloomFilter<Integer> bf = BloomFilter.create(Funnels.integerFunnel(), total, 0.001);

    public static void main(String[] args) {
        // 初始化1000000条数据到过滤器中
        for (int i = 0; i < total; i++) {
            bf.put(i);
        }

        // 匹配已在过滤器中的值，是否有匹配不上的
        for (int i = 0; i < total; i++) {
            if (!bf.mightContain(i)) {
                System.out.println("有坏人逃脱了~~~");
            }
        }

        // 匹配不在过滤器中的10000个值，有多少匹配出来
        int count = 0;
        for (int i = total; i < total + 10000; i++) {
            if (bf.mightContain(i)) {
                count++;
            }
        }
        System.out.println("误伤的数量：" + count);
    }

}
复制代码
运行结果：



img


运行结果表示，遍历这一百万个在过滤器中的数时，都被识别出来了。一万个不在过滤器中的数，误伤了320个，错误率是0.03左右。

看下BloomFilter的源码：

public static <T> BloomFilter<T> create(Funnel<? super T> funnel, int expectedInsertions) {
        return create(funnel, (long) expectedInsertions);
    }  

    public static <T> BloomFilter<T> create(Funnel<? super T> funnel, long expectedInsertions) {
        return create(funnel, expectedInsertions, 0.03); // FYI, for 3%, we always get 5 hash functions
    }

    public static <T> BloomFilter<T> create(
          Funnel<? super T> funnel, long expectedInsertions, double fpp) {
        return create(funnel, expectedInsertions, fpp, BloomFilterStrategies.MURMUR128_MITZ_64);
    }

    static <T> BloomFilter<T> create(
      Funnel<? super T> funnel, long expectedInsertions, double fpp, Strategy strategy) {
     ......
    }
复制代码
BloomFilter一共四个create方法，不过最终都是走向第四个。看一下每个参数的含义：

funnel：数据类型(一般是调用Funnels工具类中的)

expectedInsertions：期望插入的值的个数

fpp 错误率(默认值为0.03)

strategy 哈希算法(我也不懂啥意思)Bloom Filter的应用

在最后一个create方法中，设置一个断点：



img




img


上面的numBits，表示存一百万个int类型数字，需要的位数为7298440，700多万位。理论上存一百万个数，一个int是4字节32位，需要481000000=3200万位。如果使用HashMap去存，按HashMap50%的存储效率，需要6400万位。可以看出BloomFilter的存储空间很小，只有HashMap的1/10左右

上面的numHashFunctions，表示需要5个函数去存这些数字

使用第三个create方法，我们设置下错误率：

private static BloomFilter<Integer> bf = BloomFilter.create(Funnels.integerFunnel(), total, 0.0003);
复制代码
再运行看看：



img


此时误伤的数量为4，错误率为0.04%左右。



img


当错误率设为0.0003时，所需要的位数为16883499，1600万位，需要12个函数

和上面对比可以看出，错误率越大，所需空间和时间越小，错误率越小，所需空间和时间约大

常见的几个应用场景：

cerberus在收集监控数据的时候, 有的系统的监控项量会很大, 需要检查一个监控项的名字是否已经被记录到db过了, 如果没有的话就需要写入db.

爬虫过滤已抓到的url就不再抓，可用bloom filter过滤

垃圾邮件过滤。如果用哈希表，每存储一亿个 email地址，就需要 1.6GB的内存（用哈希表实现的具体办法是将每一个 email地址对应成一个八字节的信息指纹，然后将这些信息指纹存入哈希表，由于哈希表的存储效率一般只有 50%，因此一个 email地址需要占用十六个字节。一亿个地址大约要 1.6GB，即十六亿字节的内存）。因此存贮几十亿个邮件地址可能需要上百 GB的内存。而Bloom Filter只需要哈希表 1/8到 1/4 的大小就能解决同样的问题。

总结
布隆过滤器主要是在回答道缓存穿透的时候引出来的，文章里面还是写的比较复杂了，很多都是网上我看到就复制下来了，大家只要知道他的原理，还有就是知道他的场景能在面试中回答出他的作用就好了。


作者：敖丙
链接：https://juejin.im/post/5db69365518825645656c0de
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



############################################################################


