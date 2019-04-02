## java 问题定位工具清单
> 摘抄自云栖社区，来源于阿里内部技术论坛的一篇文章，原文地址 [c-云栖-红魔七号](https://yq.aliyun.com/articles/69520?utm_content=m_10360) 
另一个整合[c-菜鸟要飞公众号](https://mp.weixin.qq.com/s?__biz=MzA3ODg3OTk4OA==&mid=2651090865&idx=2&sn=49df190a6d63ab3c38ec162c45430263&chksm=844cc22ab33b4b3c5f9dd2466091defc8f6daf309a9503e97a7c4227e6d0e17d8f0fa21bbf8b&scene=0&xtrack=1&key=564d101db0e9f85443761feb7f989f9eccd54253bf875ebea01c5236147e89a3b6014e7b96bab3fd1675fcf44569103e5481b04defd9ba63f97e801b916994b044017b336b79cc0e85fd459c9cc01110&ascene=14&uin=MjUwOTQxMzEzNw%3D%3D&devicetype=Windows+7&version=62060739&lang=zh_CN&pass_ticket=FgzL0kxU%2FlTt5o4WLY9f7TZKnhKdg3TcFyAvh2wtmDrUYosX%2F8dctgqbhVC%2FnxYE)

> 整体分为几个大类，如linux命令类，排查工具btrace，eclipse插件eclipseMAT，idea插件key promoter，maven helper,java环境自带的查看jvm的命令，以及其他

### linux命令

### btrace: 线上，预发排查问题工具

类似loadrunner的脚本或者java的controller上生命的注解，通过编写脚本对class类中的方法进行监听，获取需要的数据和信息,下面列举简答使用demo,根据demo,自行拓展可以配置的参数

+ 经过观察，1.3.9的release输出不稳定，要多触发几次才能看到正确的结果
+ 正则表达式匹配trace类时范围一定要控制，否则极有可能出现跑满CPU导致应用卡死的情况
+ trace脚本使用需要先测试，否则可能导致生产环境jvm down掉
+ 由于是字节码注入的原理，想要应用恢复到正常情况，需要重启应用。
```
// 测试目标类
public class Calculator {
    private int c = 1;

    public int add(int a, int b) {
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return a + b;
    }
}

// 测试代码 具体参数，配置方法百度
@BTrace
public class BTraceTest {
    private static long count;
    static{
        println("---------------------------JVM properties:---------------------------");
        printVmArguments();
        println("---------------------------System properties:------------------------");
        printProperties();
        println("---------------------------OS properties:----------------------------");
        printEnv();
        exit();
    }

    @OnMethod(
            clazz = "Calculator",
            method = "add",
            location = @Location(Kind.RETURN)
    )
    public static void trace1(int a, int b, @Return int sum) {
        println("trace1:a=" + a + ",b=" + b + ",sum=" + sum);
        //trace1:a=2,b=6,sum=8
    }
    
    @OnMethod(
            clazz = "Calculator",
            method = "add",
            location = @Location(Kind.RETURN)
    )
    public static void trace2(@Duration long duration) {
        // 执行时间 纳秒 秒
        println(strcat("duration(nanos): ", str(duration)));
        println(strcat("duration(s): ", str(duration / 1000000000)));
    }
    
    @OnMethod(
            clazz = "Calculator",
            method = "add",
            location = @Location(value = Kind.CALL, clazz = "/.*/", method = "sleep")
    )
    public static void trace3(@ProbeClassName String pcm, @ProbeMethodName String pmn,
                              @TargetInstance Object instance, @TargetMethodOrField String method) {
        // trace3追踪Calculator类的add方法，并且追踪add方法中的任何类的sleep方法
        println(strcat("ProbeClassName: ", pcm));
        println(strcat("ProbeMethodName: ", pmn));
        println(strcat("TargetInstance: ", str(instance)));
        println(strcat("TargetMethodOrField : ", str(method)));
        println(strcat("count: ", str(++count)));
    }
    
     @OnTimer(6000)
    public static void trace4() {
        // 每隔6秒打印count值
        println(strcat("trace4:count: ", str(count)));
    }
    
    @OnMethod(
            clazz = "Calculator",
            method = "add",
            location = @Location(Kind.RETURN)
    )
    public static void trace5(@Self Object calculator) {
        // 获取Calculator类的c属性的值
        println(get(field("Calculator", "c"), calculator));
    }
    
     @OnTimer(4000)
    public static void traceMemory() {
        // 每隔4秒打印一次堆和非堆内存信息
        println("heap:");
        println(heapUsage());
        println("no-heap:");
        println(nonHeapUsage());
    }
    
     @OnTimer(4000)
    public static void trace6() {
        // 每隔4秒检测是否有死锁产生，并打印产生死锁的相关类信息、对应的代码行、线程信息
        deadlocks();
    }
}
```

### Greys
sc -df xxx: 输出当前类的详情,包括源码位置和classloader结构
trace class method: 相当喜欢这个功能! 很早前可以早JProfiler看到这个功能。打印出`当前方法调用的耗时情况，细分到每个方法`

### javOSize
通过修改字节码修改内容，即时生效，方便直接写日志，前提需要知道自己在干什么

### JProfiler
之前判断许多问题要通过JProfiler，但是现在Greys和btrace基本都能搞定了； 生产环境一般隔绝网络，一般不怎么用

### eclipseMAT
eclipse 插件

### key promoter
idea插件 快捷键提示插件

### java自带jvm分析命令

### maven helper
idea插件 以树形结构查看maven依赖，快速找到冲突jar包，解决冲突

### VM options
1. -XX:+TraceClassLoading 类从哪里来
 结果形如[Loaded java.lang.invoke.MethodHandleImpl$Lazy from D:\programme\jdk\jdk8U74\jre\lib\rt.jar]
2. 应用挂了输出dump文件
 -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/home/admin/logs/java.hprof

### jar包冲突

### dmesg 如jvm进程消失且没有痕迹场景

### RateLimiter
控制QPS在一定时间内响应帮助工具
