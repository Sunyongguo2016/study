 1. threadLocal 每个线程，独自开辟一块空间，线程独自使用； 一般用法是，在线程内设置全局变量，然后线程任意地方都可以访问到,可以用来保存线程上下文信息； 
 
 如Spring的事务管理，用ThreadLocal存储Connection，从而各个DAO可以获取同一Connection，可以进行事务回滚，提交等操作；
 
 如 项目中 TenantFactory private static ThreadLocal<BaseVO> basevo, basevo.set(); basevo.get() basevo.remove();
 ThreadLocal<Integer> tlocal, tlocal.set(), tlocal.get(), tlocal.remove();
              
 
 2. threadLocal 无法解决线程间共享对象更新问题，因为是线程独有的，（另外建议用static修饰，这样线程内所有实例，都可以共享这个线程内的全局变量 alibaba）；
 
 3. threadLocal 一般用static修饰，然后不用的时候再finally ,threadLocal.remove() 清空，回收内存； 如果没有static修饰，由于threadLocal    是 map<key,value>结构，内部key是弱引用的，只能存活在下次gc之前；所以jvm内存回收可能把内存回收走，导致出现bug, 网上有许多这样的场景；
 
 4. 一个threadlocal 只能有一个object对象，所以要用多个，需要使用多个threadLocal 修饰对象；
 
 
