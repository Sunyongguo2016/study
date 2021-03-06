## 设计模式

> 欺骗大脑，大脑比较懒散对于日常内容不会主动记忆，读书时集中注意力，如同老虎在前，刺激大脑；另外一种大脑记忆方式是重复性记忆，大脑会觉得比较重要也会记住；

> 另外大脑有个特性，对于交谈性的内容会主动思考，如2人交谈会进入场景，这个特性对于读书也适用，所以本书的特点是用图教多而且以交谈的方式讲述，让大脑集中注意进入场景； 相对正常的填鸭式生板的内容，效果更优；

> 通过本书，可以体会理解到OO思想； 很值得学习提升；最基本的点是一个类class； 任何模式都是对类关系的整合，形成灵活动态的效果。

> 这是一本教你设计一套面向对象程序的书，更好的理解oo思想。 可以自己把生活中的需求设计成oo程序； 不同于日常工作，使用已有的程序..

> 大纲
>> 本书共14章，每章介绍几个设计模式，涵盖23个设计模式，依次介绍Strategy,Observer,Decorator,Abstract Factory, Factory Method, Singleton, Command,Adapter, Facade, Template method, Iterator,
Composite,State,Proxy. 最后3章较特别，12章介绍如何将2个设计模式结合成一个（如MVC）。13介绍如何发觉新的设计模式等主题；14很快浏览了剩下的那些设计模式。

>> 第1到9章陆续介绍了9个oo原则，每个设计模式背后都包含了1个或几个设计原则。 我们设计程序，可以回归设计模式去找灵感。 oo原则是我们的目标，设计模式是我们的做法。

> 面向对象的思维方式，在执行一步一步动作时，通过自身对象，调用自身一层一层方法执行，涉及到其他对象，可能把该对象封装都自身属性，然后通过方法调用其他对象的某个行为；

* [1.设计模式入门-策略模式](#1)
* [2.观察者模式](#2)
* [3.装饰模式](#3)
* [4.工厂模式](#4)
* [5.单例模式](#5)
* [6.命令模式](#6)
* [7.适配器模式与外观模式](#7)
* [8.模板方法模式](#8)
* [9.迭代器与组合模式](#9)
* [10.状态模式](#10)
* [11.代理模式](#11)
* [12.复合模式](#12)
* [13.真实世界中的模式](#13)
* [14.介绍剩下的模式](#14)

<h3 id="1">设计模式入门-策略模式</h3>

> 下面通过Duck类来举例，递进式的介绍设计原则，逐步介绍出好的设计效果

* 1.1 找出应用中可能需要变化的部分，独立出来，不要和那些不需要变化的代码混合在一起；Duck类的fly,quack行为是抽出来的相对来说变化的部分

* 1.2 针对接口编程，而不是针对实现编程；针对接口编程，最大的好处是动态化，类似多态的效果； Duck抽取出Fly, Quack类 FlyBehavior 有多种实* 现，可以给

* 1.3 多用组合，少用继承
	解释：Duck() 的行为有fly, quack等 用继承的方式，每个类都要改动；用组合方式，把fly,quack作为对象，然后fly对象再采用接口类赋值，具体使用时fly通过多态实现自身的独特性，组合的方式比继承更灵活，动态化；

 * 1.4 *策略模式*
> 策略模式 定义了算法族，分别封装起来，让它们之间可以互相替换，此模式让算法的变化独立于使用算法的客户。 算法族（flyBehavior,quackBehavior,内部有多种实现）

 * 1.5 总结
> 第一章介绍oo基础： 抽象，封装，继承，多态； oo原则：封装变化，多用组合，少用继承；针对接口编程，不针对实现编程；
> oo模式：策略模式，定义算法族，分别封装起来，让它们之间可以互相替换，此模式让算法的变化独立于使用算法的客户； 理解就是把自己的一些行为，用接口类代替，实现动态执行行为或算法；


<h3 id="2">观察者模式</h3>

> 观察者模式可以帮对象了解现状，不会错过感兴趣的事；对象甚至在运行时可决定是否要继续被通知。 观察者模式是jdk中使用最多的模式之一，非常有用。有了观察者模式，你将会消息灵通。

> 类似报纸订阅，出版者(Subject)+订阅者(Observer)=观察者模式; 观察者模式：定义了对象之间的一对多依赖，这样一来，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新；
（观察者模式定义了一系列对象之间的一对多关系，当一个对象改变状态，其他依赖者都会收到通知） 

 * 2.1 设计原则：为了对象之间松耦合设计而努力

松耦合设计的oo系统更有弹性，观察者模式中，subject, 和observe之间就是松耦合， observe只需要订阅并实现接口就可以接受消息，然后取消订阅就不用接受消息； subject 维护一个订阅者列表进行通知，不用改变自己代码； 实现松耦合；

观察者模式中，Subject中存有Observe对象的引用，Observe中有Subject的引用，但是互相耦合不影响，Subject 在改变数据后（set方法末尾），调用observe.update(); Observe中存有Subject引用，


 * 2.2 java utils

自己可以设计实现一个 观察者模式； 另外java 包也提供了实现， java.util.Observer 是一个接口，作为观察者；
java.util.Observable 是一个类，通过继承，作为被观察者（subject）; 主要方法有addObserver() deleteObserver() notifyObservers();

 * 2.3 小结
oo原则： + 为交互对象之间的松耦合而努力

oo模式： 观察者模式，在对象之间定义一对多依赖，这样一来，当一个对象改变状态，一来它的对象就会收到通知并自动更新（以松耦合方式在一系列对象之间沟通状态，代表人物--mvc;）

观察者模式，类比出版-订阅的关系； 订阅者需要订阅或取消订阅，然后才可以接收到消息

<h3 id="3">装饰模式</h3>

> 本章会学到如何使用用对象组合的方式，做到在运行时装饰类，一旦熟悉了装饰的技巧，可以在不修改任何底层代码的情况下，给对象赋予新的职责。

> 购买一杯咖啡，需要蒸奶，豆浆，摩卡，2倍红豆这些配料； 然后需要计算价格cost()， 描述最后的结果description()

 * 3.1 设计原则：类应该对拓展开放，对修改关闭；（如写功能，不改变旧代码，防止不稳定；新功能封装成一个方法调用；）

 * 3.2 装饰者模式

装饰者模式动态地将责任附加到对象上，若要拓展功能，装饰者提供了比继承更有弹性的替代方案； 装饰者和被装饰者有相同的超类型，可以用多层包装，可以在任何时候
被装饰，所以可以运行时动态地，不限量地用自己喜欢的装饰者来装饰对象。

```
public class Mocha extends CondimentDecorator {
 Beverage beverage;
 public Mocha(Beverage beverage) {
 this.beverage = beverage;
 }
 public String getDescription() {
 return beverage.getDescription() + ", Mocha";
 }
 public double cost() {
 return .20 + beverage.cost();
 }
}
// 装饰者模式，自身嵌套，不断装饰自身； 层层嵌套，不断调用自身的getDescription() cost()方法；
```

 * 3.3 典型案例

java.io 内的许多类都是装饰者，应用了装饰者模式； LineNumberInputStream(BufferedInputStream(FileInputStream))

LineNumberInputStream也是一个具体的“装饰者”。它加上了计算行数的能力;

BufferedInputStream是一个具体的“装饰者”，它加入两种行为：利用缓冲输入来改进性能；用一个readLine()方法（用来一次读取一行文本输入数据）来增强接口

BufferedInputStream及LineNumberInputStream都扩展自FilterInputStream，而FilterInputStream是一个抽象的装饰类

 * 3.4 小结
> 对拓展开放，对修改关闭； 装饰者模式--动态地将责任附加在对象上，想要拓展功能，装饰者提供有别于继承的另一种选择。


<h3 id="4">工厂模式</h3>

> 除了new以外，还有更多制造对象的方法，这里将了解到实例化这个活动不应该总是公开的进行，初始化也会造成耦合的问题；工厂模式可以从复杂的依赖中解困。
 另外 工厂模式可以把创建对象的代码用方法‘围’起来，不会让创建的代码太零散。 而且，使用工厂模式生成对象，可以减少使用对象的类PizzaStore对 创建对象语句的依赖，（只要子类继承，实现抽象创建对象的方法即可）
 
> 1.需要制作pizza, 进而制作不同口味的pizza,if(type)来解决；简单工厂模式解决 2. 每个加盟店有自己各自区域的风格，如芝加哥，纽约风格的pizza;抽象工厂方法解决,具体子类把抽象工厂方法具体实现，返回实例pizza; 3.pizza对原料很挑剔，需要不同地区的原料，此时用到抽象工厂模式，抽象工厂生成多种原料，对应多个abstract 方法，每个地区实现自己的原料；

* 4.1 简单工厂模式使用

PizzaStore{ SimplePizzaFactory factory; factory.createPizza(type)}    ----SimplePizzaFactory. createPizza( // 根据类型创建Pizza对象) // 理解：工厂模式更加动态化，解耦；创建出对象然后具体的类，对对象执行自己需要的操作；

//另一种工厂模式 ，更精细化； 父类抽象方法获得对象，子类负责具体实现；
abstract Product factoryMethod(String type)

* 4.2 工厂模式

所有工厂模式都用来封装对象的创建，工厂方法模式通过让子类决定该创建的对象是什么。来达到将对象创建的过程封装的目的。

工厂模式 定义了一个创建对象的接口（比如一个抽象的 “工厂方法” factoryMethod()返回产品），但由子类决定要实例化的类是哪一个，工厂方法让类把实例化推迟到子类。

工厂方法让子类‘决定’要实例化的类是哪一个； 即运行时才动态的返回对应的Product,选择了哪个子类，自然就决定了实际创建的产品是什么;

* 4.3 oo原则：依赖倒置原则

要依赖抽象，不要依赖具体类。 （使用抽象类调用方法，利用多态实现具体的行为，而不是通过具体类，要抽象！！！），要实现抽象倒置原则，工厂方法是最有威力的技巧之一。

遵循次原则的指导技巧： 1.变量不可用持有具体类的引用。2.不要让类派生自具体类 3.不要覆盖基类中已经实现的方法。

* 4.4 抽象工厂模式

> 抽象工厂模式提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类。

`抽象工厂方法经常以工厂方法的方式实现。` 工厂方法创建对象使用的是继承，抽象工厂创建对象使用的是组合。 
工厂方法，通过继承，然后子类返回对象，主要是解耦； 抽象工厂不仅是解耦，另一个优点是把一群相关的产品集合起来，比如pizza原料工厂的抽象方法

` 工厂模式，就是用来创建对象的，任何对象创建 都可以用工厂模式`

> 抽象工厂： 我是抽象工厂，当你需要创建产品家族，和想制造相关产品集合起来时，你可以使用我

> 工厂方法： 我是工厂方法，我可以把你的客户代码从需要实例化的具体类中解耦。（创建对象，在子类中实现，比如创建对象需要一些判断条件，此时就能比较出效果）； 只要把我继承成子类，并实现我的工厂方法就可以了。

* 4.5 小结
 
 * 抽象工厂模式--提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类。
 * 工厂方法模式--定义了一个创建对象的接口，但由于类决定要实例化的类是哪一个，工厂方法让类把实例化推迟到子类；
 * 这2个模式可以将对象的创建封装起来，以便得到更松的耦合，更有弹性的设计，工厂是很有威力的技巧，指导我们依赖抽象编程，而不是针对实现类编程。
 
<h3 id="5">单例模式</h3>

* 5.1 单例模式定义

单例模式确保类只有一个实例，并提供一个全局访问点。 类自己管理一个单独实例，避免其他类再自行产生实例，想取得单件实例，通过单件类是唯一途径。

* 5.2 同步下的单例

同步一个方法可能造成程序执行效率下降100倍，因此，getInstance()程序使用在频繁运行的地方，可能要重新考虑了。

```
// 饿汉式创建实例 
public class Singleton{
	private static Singleton uniqueInstance = new Singleton();
	private Singleton(){}
	public static Singleton getInstance(){
		return uniqueInstance;
	}
}
// 利用这个做法，依赖JVM在加载这个类时马上创建此唯一的单件实例。JVM保证在任何线程访问uniqueInstance静态变量之前，一定先创建实例。


// 双重检查加锁 dcl 方式； 懒汉模式下的线程安全单例；
public class Singleton{
	private volatile static Singleton uniqueInstance;
	private Singleton(){}
	public static Singleton getInstance(){
		if(uniqueInstance == null){
			synchronized (Singleton.class){
				if(uniqueInstance==null){
					uniqueInstance = new Singleton();
				}
			}
		}
	}
}
// 异常DCL方式 只在1.4以上版本适用； 如果性能是你关注的重点，那么这个做法可以帮你大大减少getInstance()的时间耗费。
```

* 5.3 小结

单例模式--确保一个类只有一个实例，并提供全局访问点； 另外注意一点是，如果多个类加载器，会产生多个实例；

<h3 id="6">命令模式</h3>

> 本章我么将把封装带到一个全新的境界，把方法调用封装起来；通过封装方法，可以做一些很聪明的事，如记录日志等；
命令模式，可以将‘动作的请求者’从‘动作的执行者’对象中解耦；

> 通过遥控器，控制7个具体的插槽， 这个模式主要有3种类，遥控器类，命令类，产品类；遥控器执行自己的命令行为，如开，关，并传入开关对应的位置；通过位置找到命令对应实例（作为实例存在遥控器属性上），执行on,off指令；
 此时到了command类，会调用通用的execute()方法，execute执行具体的产品的开关命令，（产品作为实例存在命令的实现类上）；  最后产品对应的product.on()被execute 调用到；
 
* 6.1 命令模式

命令模式将‘请求’封装成对象，以便使用不同的请求，队列或者日志来参数化其他对象。命令模式也支持可撤销的操作。 （把命令作为一个类）

命令模式，把命令Command当做一个对象，把接受者receive作为对象传到Command中，command包含receive; command.execute()，执行接受者的具体命令；

关键类3个， Command类（包含receive实例，execute方法）；receive类（包含action｛｝方法，具体执行什么）；另外一个类是RemoteController类，控制器类，（存储多个命令实例，调用命令执行方法如 onButtonWasPushed(int slot){commands[slot].execute();}）

NoCommand implements Command;  NoCommand 对象是一个空对象（null object）例子，当不想返回一个有意义的对象时，空对象就很有用。 许多设计模式会看到空对象的使用，有些时候空对象本身也是一种设计模式。


* 6.2 撤销命令

```
public interface Command{
	public void execute(){};
	public void undo();
}
```
execute()执行命令， undo()撤销命令； undo（执行具体实现类Command的相反即可）； 然后更高级的撤销命令是有状态位，判断的撤销命令，如三级风扇的撤销；

宏命令： 宏命令是一系列命令的集合； 可以通过MacroCommand implements Command{Command[] commands;} 来实现宏命令，宏命令同样是命令，内部有一系列命令，然后execute{}可以遍历执行一系列命令； undo{}遍历取消所有命令

* 6.3 命令模式的更多用途：队列请求；

工作队列类和进行计算的对象之间是完全解耦的。此刻线程在进行财务运算，下一刻在读取网络数据；工作队列不知道线程在做什么只是读取命令对象，然后执行其execute方法；类似地，他们只要
是实现命令模式的对象，就可以放入队列中，当线程可用时，就调用此对象的execute（）方法；

日志请求：

某些应用需要将我们所有操作记录到日志中，并能在系统死机后，再次启动重新调用这些动作回复之前的状态； 我们执行命令的时候，将历史记录从磁盘读取出来，将命令对象重新加载，并成批地依次调用这些队形的execute方法；
这种方式可以拓展到事务中，不必记录每次执行的数据结果，而是记录命令，一整群操作必须全部完成，或者没有进行任何的操作。 `每个命令被执行时，execute{store(),load()}`会存储然后执行；这样系统死机后也可以重新加载命令，正确执行；

* 6.4 小结

> 在本节，我们加入一个模式，允许我们将动作封装成命令对象，这样一来就可以随心所欲地存储，传递和调用它们。

> 命令模式将发出请求的对象和执行请求的对象解耦；通过命令对象沟通，共3种类；‘调用者’通过调用'命令对象'的execute发出请求，这会使得‘接受者’的动作被执行； 另外还有撤销命令，宏命令；命令也可以用来实现日志和事务系统；

> 命令模式，将请求封装成对象，这可以让你使用不用的请求，队列，或者日志请求来参数化其他对象。命令模式也支持撤销，宏命令；

<h3 id="7">适配器与外观模式</h3>

> 适配器模式：想要把三角形的图形，拼接到半圆的图形上，实际上这2个接口无法匹配，中间通过一个适配器，一半接受三角形，另一半作为半圆形，将2块内容拼接起来；

> 外观模式：观赏电影，不需要对每个产品依次打开，可以通过一个类，简化接口，watchMovie()然后执行观看电影；

* 7.1 适配器模式

适配器 模式 将一个类的接口，转换成客户期望的另一个接口。适配器让原本接口不兼容的类可以合作无间。主要是通过创建适配器来进行接口转换， Adapter适配器，实现了目标接口，并持有被适配者的实例。适配器，可以想象成同声翻译，或者多空插头适配器；

java中通过组合的方式，将一个类作为对象，然后转换出目标接口的接口方法； 实现适配器效果；


* 7.2 外观模式

外观模式，让接口更简单；通过实现一个提供更合理的接口的外观类，可以将一个复杂的子系统变得容易使用。 外观可以包装许多类，目的是简化接口，方便使用；比如遥控器watchMovie();自动调用了几个产品类的接口，简化了工作。

外观模式，我们创建一个接口简化而统一的类，而来包装子系统中一个或多个复杂的类。定义：`外观模式`提供了一个统一的接口，用来访问子系统中的一群接口。外观定义了一个高层接口，让子系统更容易使用。

* 7.3 设计原则：最少知识原则

最少知识原则：只和朋友交谈。 就是要减少对象之间的交互，不要让太多类耦合在一起；减少交互和依赖的类，避免多层次类的调用；
（如果许多类之间相互依赖，那么这个系统就会变成一个易碎的系统，它需要花许多成本去维护，也会因为太复杂而不太容易被其他人了解）

* 7.4 小结

> 本章加入几个模式后，可以改变接口，并降低客户和系统之间的耦合。 增加设计原则：只和朋友交谈，在类中只调用和自身关系最亲密的引用类型的方法；

> 适配器模式，用于转换接口； 将一个类的接口，转换成客户期望的另一个接口，适配器让原本不兼容的类可以操作无间；

> 外观模式，提供了一个统一的接口，用来访问子系统中的一群接口。外观定义了一个高层接口，让子系统更容易使用。

<h3 id="8">模板方法模式</h3>

> 举例子，冲泡咖啡因饮料， 1.加沸水 2.冲泡 3.倒入杯中 4.加作料； 按照这个算法模板作为基类，子类冲咖啡，冲茶 对2，4 进行具体的实现；

 * 8.1 认识模板方法
 
 模板方法定义了一个算法的步骤，并允许子类为一个或多个步骤提供实现；
 
 模板方式模式：在一个方法中定义了一个算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以在不改变算法结构的情况下，重新定义算法中的某些步骤。 (这里的一个算法 肯能包括多个步骤,可以有任意几个抽象的步骤，由子类来具体实现。)
 
 * 8.2 hook 钩子
 
 在模板算法中； 可以在一系列算法最后，执行hook() 方法， 在基类只是一个什么都不做的方法，在子类却可以实现自己需要的逻辑（灵活，拓展性）。 hook 经常出现在多种编程语言中！！！
 
 hook 钩子的作用；  hook 竟然能够作为条件控制，影响抽象类中的算法流程； 许多语言都可以看到hook的影子（hook，可以做逻辑控制，控制模板算法的流程）
 
 * 8.3 设计原则：好莱坞原则
 
 别调用我们，我们会调用你；

 模板方法模式是一个很常见的模式，到处都是，这个模式很常见是因为对于创建框架来说，这个模式非常棒。由框架控制如何做事情，而由用户指定框架算法中每个方法的实现细节。
 
 模板方法模式在实际使用，可能不是很符合规范，简单理解就是按照这个思想，具体实现有一些差异化。
 
 * 8.4 小结
 > 别找我，我会找你 (由超类主控一切，当他们需要的时候，自然会去调用子类，这就跟好莱坞一样)
 
 > 模板方法模式--在一个方法中定义一个算法的骨架，而将一些步骤延续到子类中。模板方法使得子类可以在不改变算法结构的情况下，重新定义算法中的某些步骤。
 
<h3 id="9">迭代器与组合模式</h3>

> 背景：煎饼屋和对象餐厅合并了，要出一份菜单，但是煎饼屋是数据，对象餐厅是arraylist; 双方都不希望改自己的实现，因为依赖太多了。所以合并菜单遇到了麻烦；

> 迭代器模式提供一种方法顺序访问一个聚合对象中的各个元素，而又不暴露其内部的表示；不用了解集合对象的内部的存储结构；

> 菜单新增了菜单项，新增子菜单； 所以菜单就组成了树形结构，需要改菜单的数据结构，此时到了组合模式

> 组合模式 允许你将对象组合成树形结构来表现“整体/部分”层次结构。组合能让客户以一致的方式处理个别对象以及对象组合。

_使用组合模式让我们能用树形方式创建对象的结构，树里面包含了组合以及个别的对象 _  

树节点，和叶子都实现了一个接口A； 我们通过接口A的方法，调用方法，实现对具体
的不同的子节点的操作； 这样就屏蔽了 树节点，结构不一致的细节；

通过调用抽象方法调用我们像要的对象； 如增加节点，调用addItem; removeItem; print; 对于叶子节点类型
的对象，有意义的只有print, add ,remove 不需要调用；  接口A 就扮演了多个角色；



<h3 id="10">状态模式(事物的状态帮助对象控制自己的行为)</h3>

  * 10.1 案例
 
> 背景：糖果机，通过状态，控制是否需要投币，投币后通过状态，发放出糖果； 案例背景是一个糖果机，类似地铁站的投票吐饮料的饮料机； 通过状态，控制机器的各种行为；

流程调用过程，4级流程的转换，其实就是用到了初级的状态模式(有int表示状态，真正的状态模式是类表示状态)；

状态模式； 把状态设计为接口，然后接口里有所有的动作，实现类都对这些动作做出自己状态相应的操作；

  * 10.2 状态模式定义
 
状态模式定义：状态模式允许对象在内部状态改变时改变他的行为，对象看起来好像修改了他的类； 具体理解（把自己内部的状态抽象为接口；然后具体行为，通过State接口执行行为，
接口的每个继承对应了不同的执行行为。这样就省去了if,else，通过接口控制不同的执行逻辑（即对象的具体行为动作））
  
  * 10.3 策略模式 与 状态模式
 
 状态模式：封装基于状态的行为，并将行为委托到当前状态
 
 策略模式：将可以互换的行为封装起来，然后使用委托的方法，决定使用哪一个行为
 
 * 10.4 小结
 
 * 状态模式允许一个对象基于内部状态而拥有不同的行为。
 * 和状态机（psm）不同，状态模式不是用一个简单类型表示状态，而是通过类来表示状态
 * context会将行为委托给当前状态对象（context表示当前的状态），状态模式允许context随着状态的改变而改变行为。
 * 使用状态模式会通常使设计中的类的数目大量增加



<h3 id="11">代理模式</h3>

> 代理可以做的： 控制和管理访问。代理就是代表某个真是的对象； 案例：客户堆通过RMI远程调用服务器堆，通过客户堆的代理，获取远程的对象的信息；

* 11.1 RMI技术 远程调用以及实例

RMI好处在于你不必亲手写任何的网络或I/O代码，客户程序调用远程方法（真正的服务所在，发包封装了请求的方法，参数值等），就和在运行在客户自己本地的
JVM上的对象进行正常方法调用一样。

* 11.2 定义代理模式

代理模式为另一个对象提供一个替身或占位符以控制对这个对象的访问。

几种代理访问模式：

* 远程代理控制访问远程对象
* 虚拟代理控制访问创建开销大的资源
* 保护代理基于权限控制堆资源的访问

* 11.3 使用java API的代理，创建一个保护代理

java在 java.lang.reflect包中有自己的代理支持，利用这个包可以在运行时动态地创建一个代理类，实现1个或多个接口，并将方法的调用转发到自己制定的类。
因为实际的代理类是在运行时动态创建的，我们称这个java技术为：动态代理。

java创建了Proxy类，具体代码不能直接放在proxy中，放在InvocationHandle中。 InvocationHandler的工作时响应代理的任何调用。

创建InvocationHandler，（调用处理器），当代理得了方法被调用时，代理会把这个调用转发给InvoctionHandler,而是通过其中的接口invoke() 实现的。
  * invoke(Object proxy, Method method, Object[] args)
  * return method.invoke(person,args)

* 11.4 小结
+ 代理模式为另一个对象提供代表，以便控制客户对对象的访问，管理访问的方式有许多种
+ 远程代理管理客户和远程对象之间的交互
+ 虚拟代理控制访问实例化开销大的对象
+ 保护代理基于调用者控制对象方法的访问。
+ 代理在结构上类似装饰者，但目的不同，装饰者模式为对象加上行为，而代理则是控制访问。
+ java内置的代理支持，可以根据需要建立动态代理，并将所有调用分配到所选的处理器
+ 和其他包装者一样，代理会造成你的设计中的类的数目增加。


<h3 id="12">复合模式</h3>

> 模式通常在一起使用，并被组合在同一个设计解决方案中，被称为复合模式。

1. 鸭子发出叫声的案例，添加鹅的种类；（策略模式+适配器模式，鹅转换成鸭子的Quackable接口）

2. 在上面的基础上，想知道发出的叫声中，有多少是鹅发出的，应用装饰者模式

Quackable redheadDuck = new QuackCounter(new RedheadDuck());

3. 使用工厂模式生产鸭子， 让我们把创建和装饰的部分都包装起来，工厂可以生产各类不同类型的鸭子的产品家族；

4. 现在我们想要管理一群鸭子，整体管理；不必零散的处理每个鸭子的方法，可以使用组合模式，把所有的鸭子对象放到一个list<interface>里，当然组合包装的外层对象也要实现interface接口
在执行interface方法时，迭代list对象的interface方法；

5. 需要追踪个别的鸭子的行为，持续追踪个别鸭子呱呱叫的行为，使用观察者模式； 注册观察者，唤醒观察者。（在一个类中实现注册和通知，可以观察一个类的行为）
观察者模式，可以在观察者，和被观察者之间，互相引用； 实现互相通信； （理解对象直接的一种通信）

```
总结下：我们做了什么？

我们从一堆Quackable开始...

有一只鹅出现了，它希望自己像一个quackable.
所以我们利用了适配器模式，将鹅适配成了quackable.现在你就可以调用鹅适配器的quack()方法让鹅咯咯叫。

然后，呱呱叫学家决定要计算呱呱叫声的次数。
所以我们使用装饰者模式，添加了一个名为QuackCounter的装饰者。它用来追踪quack()被调用的次数，并将调用委托给它所装饰的quackable对象；

但是呱呱叫学家担心他们忘记了加上quackCounter装饰者。
所以我们使用抽象工厂模式创建鸭子。从此以后，当他们需要鸭子时，就直接从工厂创建，工厂会给他们装饰过的鸭子。（如果要没有装饰过的额鸭子，从另一个工厂取）

此时，又有鸭子，又有鹅，又是quackable的....我们有管理上的困扰。
所以我们需要使用组合模式，将许多quackable集结成一个群。 这个模式也允许群中有群，以便让呱呱叫家来管理鸭子家族。我们在实现中通过使用ArrayList的java.util的
迭代器而使用了迭代器模式。

当任何呱呱叫声响起时，呱呱叫学家都希望能被告知。
所以我们使用观察者模式，让呱呱叫学家注册成为观察者。现在，当呱呱声响起时，呱呱叫学家就会被通知了，在这个实现中，我们再度用到了迭代器。呱呱叫学家不仅
可以当某个鸭子的观察者，甚至可以当一个整群的观察者。

```


> MVC是数个设计模式的整合，结合了观察者模式、策略模式和组合模式

* 小结：

* 复合模式结合2个以上的模式，组成一个解决方案。解决一再发生的一般性问题。
* mvc是复合模式，结合了观察者模式、策略模式和组合模式
* 模型使用观察者模式，以便观察者更新，同时保持2者之间解耦。
* 控制器是视图的策略，视图可以使用不同的控制器实现，得到不同的行为。
* 适配器模式用来将新的模型适配成已有的视图和控制器

<h3 id="13">真实世界中的模式</h3>

 * 13.1 模式与描述 （将已雪的模式，和描述连接）

  装饰者模式： 包装一个对象提供新的行为；
 
  状态模式：封装了基于状态的行为，并使用委托在行为之间切换
  
  迭代器模式：在对象的集合之间游走，而不暴露实现
  
  外观模式：简化一群类的接口
  
  策略模式：封装可以互换的行为，并使用委托来决定要使用哪一个
  
  代理模式：包装对象，以控制对此对象的访问
  
  工厂模式：由子类决定要创建的类具体是哪一个
  
  适配器模式：封装对象，并提供不同的接口
  
  观察者模式：让对象能够在状态改变是被通知
  
  模板方法：由子类决定如何实现一个算法中的步骤
  
  组合模式：客户用一致的方式处理对象集合和单个对象
 
  单例模式：确保只有一个对象被创建
  
  抽象工厂：允许客户创建对象的家族，而无需指定他们的具体类
  
  命令模式：封装请求称为对象
 
 * 13.2 模式分类(23种)
  
  创建型：涉及到将对象实例化，将客户从所需要实例化的对象中解耦
  
  Singleton; abstract factory; factory method/ builder; prototype; 
  
  行为型：涉及到类和对象如何交互及分配职责
  
  Template method; iterator; command; Observer; state; strategy; / visitor, mediator, memento, interpreter, chain of responsibility 
  
  结构型： 可以将类或对象组合到更大的结构中。
  
  Decorator, proxy,composite, facade,adapter; / bridge,flyweight
 

 * 13.3 小结要记
 
 请牢记：使用适合自己的模式，也可以自己创造，灵活运用，但是你所遇到的大多数的模式都是现有模式的变体，而非新的模式。
 
 模式能够为你带来的最大好处之一是：团队拥有共享词汇，也就是专业术语。
 
 <h3 id="14">介绍剩下的模式</h3>

 * 14.1 桥接模式

 案例：设计一个遥控器的接口，然后写遥控器的实现；（桥接模式不只改变你的实现，也改变你的抽象） 桥接模式通过将实现和抽象放在两个不同的类层次中而使它们可以独立改变。
 
 适合用于在需要跨越多个平台的图形和窗口系统上。 当需要用不同的方式改变接口和实现时，你会发现桥接模式很好用。
 
 * 14.2 生成器
 
 案例：我们将游乐园的旅游规划的创建过程，封装到一个对象中。 （
 这里和自己遇到的公式引擎类似，公式，结果，计算service都作为一个对象，而内部的属性，有一套生成的构造方法，构造出计算公式对象‘涉及到task，sheet,cells的数据存放’；案例是把旅游规则的天数，hotel,tickets,封装成add方法，然后构造出一个旅游规则的对象）
 （使用生成器模式builder pattern封装一个产品的构造过程，并允许按步骤构造；）
 
 生成器的优点是将一个复杂对象的创建过程封装起来；允许对象多个步骤创造，（sethotel,setTicket,和只有一个步骤的工厂模式不同）；产品的实现可以被替换，因为客户只看到的是一个生成接口； Planner plan = builder.getPlanner(); builder里设置好了各种属性如hotel,tickets,days
 
 生成器经常被用来创建组合结构；

 * 14.3 责任链模式
 
 案例：将收到的海量邮件，通过过滤程序，让不同部门处理自己需要收到的邮件（当你想让一个以上的对象有机会能够处理某个请求的时候，就使用责任链模式）
 自己使用过的，上报操作，就是一个责任链的模式； for循环里，多个对象的handler(requestBody) 都会处理一次请求封装的对象，然后每个处理会执行对应的入库，逻辑操作；
 
 通过责任链模式，可以为某个请求创建一个对象链。每个对象依序检查此请求，并对其进行处理，或者将它传给链中的下一个对象。
 
 优点：将请求的发送者和接收者解耦；通过改变链内的成员或调动它们的次序，允许动态的新增或删除责任；

 用途： 常被用在窗口系统，点击鼠标和键盘之类的事件。另外缺点是不容易观察运行时特征，不易拍错；

 * 14.4 蝇量（Flyweight pattern）

 案例：在程序图形界面上，要画上许多的树； 即树的多个实例； 一种做法是new 许多树； 另一种做法是，树是唯一的，通过另一个对象里面维护创建数据的二维数组，表示数，对应的状态； 2个对象解决；
 
 如想要某个类的一个实例能用来提供多个“虚拟实例”，就使用该模式

 优点：减少运行时对象实例的个数，省内存； 将许多“虚拟”对象的状态集中管理；
 
 用途：当一个类有许多实例，且能被同一个方法控制的时候，可使用蝇量模式。 缺点是比较固定，由于是虚拟实例，所以单个虚拟实例无法拥有独立不同的行为。
 
 * 14.5 解释器模式
 
 使用解释器（interpreter Pattern）模式为语言创造解释器

 案例：解释器模式，可以用来解释一门语言，每个语法规则对应一个类，即每个指令，对应一个类，有相应的属性，和解释方法Interpret()方法；方法中传入一个context(上下文)，这里的上下文的概念是正在解析的语言字符串输入流；
 
 优点：每个语法规则表示为一个类，方便实现语言；由于语法是许多类组成，所以可以轻易地改变或拓展语言；在类中加新方法，可以处理解释语言的新的逻辑。
 
 用途：当你要实现一个简单语言时，可以使用解释器模式；

 * 14.6 中介者模式
 
 使用中介者模式（Mediator Pattern）来集中相关对象之间复杂的沟通和控制方式
 
 优点：通过将对象彼此解耦，增加对象复用性； 通过将控制逻辑集中，可以简化系统维护；可以让对象传递消息变得简单且大幅减少（之前多个对象互相传递消息复杂，冗余）；

 用途：中介者常用来协调相关的GUI组件；缺点是设计不当，中介者本身很复杂；
 
 * 14.7 备忘录模式
 
 当你需要让对象返回之前的状态时（例如，用户请求“撤销”），就使用备忘录模式（MementoPattern）;
 
 场景：如游戏在关卡设计中，通过每一关设计一个能复活到对象关卡数据的角色，进度停在对应的关卡上；例如对程序的某个阶段备份，也是如此；（客户只有状态，没有数据；）
 
 优点：将被储存的状态放在外面，不要和关键对象混在一起，可以维护内聚；提供了容易实现的恢复能力
 
 用途：备忘录用于储存状态； 在java中，其实可以考虑使用序列化机制储存系统状态；
 
 * 14.8 原型模式
 
 当创建给定类的实例的过程很昂贵或很复杂时，就使用原型模式（Prototype Pattern）.
 
 原型模式，允许你通过复制现有的实例来创建新的实例（java中，使用clone()，或者发序列化）；该模式重点在于，客户的代码不知道要实例化何种特定类的情况下，可以制造出新的实例（创造那个对象的逻辑，处理后，return object; 客户只接受return的object）。
 
 优点：向客户隐藏制造新实例的复杂性； 某些环境下，复制对象比创建兑现更有用；
 
 用途：在一个复杂的类层次中，系统必须从其中的许多类型创建新对象时，可以考虑原型； 缺点是对象复制有时相当复杂；
 
 * 14.9 访问者模式
 
 当你想要为一个对象的组合增加新的能力，且封装并不重要时，可使用访问者模式（visitor pattern）

 优点：访问者允许你对组合结构加入新的操作，而无需改变结构本身；想要加入新的操作，相对容易；访问者锁进行的操作，其代码是几种在一起的。
 
 用途：当采用访问者模式时，就会打破组合的封装； 因为游走的功能牵涉其中，所以对组合结构的改变更加困难；
 
