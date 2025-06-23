# 23种设计模式介绍
## 1. 创建型概述 
|模式名称|意图|适用场景|关键参与者|应用实例|
|------|------|------|------|------|
|_1.1 抽象工厂（Abstract Factory）-对象创建型_|抽象出创建一批相关对象的接口|用来创建一个产品族|AbstractFactory（工厂抽象）<br>ConcreteFactory（具体工厂类）<br>AbstractProduct（产品抽象）<br>ConcreteProduct（具体产品）<br>Client|javax.xml.parsers.DocumentBuilderFactory<br>springframework.BeanFactory|
|_1.2 生成器（Builder）-对象创建型_|把创建复杂对象的过程抽象出来|创建对象的过程复杂，且不同的创建过程对应对象的表示也不同|Builder（创建product对象过程的抽象）<br>ConcreteBuilder（具体的对象生成器）<br>Director（类似Client使用Builder的对象）<br>Product（产品）<br>Client|org.springframework.web.client.RestClient.Builder|
|_1.3 工厂方法（Factory Method）-对象创建型_|单一对象的创建延迟到子类完成|当不知道应该创建哪个产品时，将创建对象任务交给具体子类（知道应该实例化哪个产品）完成|Product（Product抽象）<br>ConcreteProduct（具体的Product）<br>Creator（创建Product的抽象）<br>ConcreteCreator（具体的创建Product的类）<br>Client|java.sql.Driver<br>sun.util.spi.CalendarProvider|
|_1.4 原型（Prototype）-对象创建型_|用克隆的方式创建新对象，而不是new|当对象状态只有几种，但是要创建大量对象实例时、且创建开销大时适用。比如迷宫的墙、门等对象，方向只有四种，可以提前创建好一种状态的对象实例，后续对象只需clone后对方向参数调整下。实现可以采用浅拷贝或深拷贝|Prototype（抽象出克隆方法的接口）<br>ConcretePrototype（实现克隆方法的具体类）|jdk的Cloneable接口（浅拷贝）<br>对象序列化（深拷贝）|
|_1.5 单例（Singleton）-对象创建型_|控制某个类在系统中只存在一个实例|对开销大的对象只创建一次<br>对需要串行访问的数据或者执行的代码，全局只有一个统一调用入口|Singleton（单例对象）|spring的BeanFactory<br>数据库连接管理器|
## _创建型UML类图_
### 1.1 抽象工厂
![](out\uml\AbstractFactory\AbstractFactory.png)
### 1.2 生成器
![](out\uml\Builder\Builder.png)
### 1.3 工厂方法
![](out\uml\FactoryMethod\FactoryMethod.png)
### 1.4 原型模式
![](out\uml\Prototype\Prototype.png)
### 1.5 单例模式
![](out\uml\Singleton\Singleton.png)
### 1.2 
## 2. 行为型
|模式名称|意图|适用场景|关键参与者|应用实例|
|------|------|------|------|------|
|_2.1 责任链（Chain of Responsibility）-对象行为型_|让多个对象实例都有机会处理某个请求|比如每个小窗口应用左上角都用帮助按钮，点击最上层一个对话框的帮助按钮，如果这个对话框不处理就将请求转发给他的上一级窗口，以此类推如果没有一个窗口处理，最终请求就会由应用本身去处理|Handler（处理器抽象）<br>ConcreteHandle（具体处理器）<br>Client（客户端）|Java Servlet Filter<br>FilterChainProxy<br>Spring HandlerInterceptor|
|_2.2 命令（Command）-对象行为型_|将请求者与请求处理者解耦|需要支持撤销、重做<br>支持命令队列<br>菜单按钮点击触发行为<br>比如将文本的复制操作作为一个命令，文本被复制到文档后，我们也可以Ctrl+z撤销复制内容，就是由command对象负责调用对应的文档对象的复制和撤销操作|Command（请求的抽象）<br>ConcreteCommand（具体的请求对象）<br>Receiver（请求的处理对象）<br>Client（客户）<br>Invoker（命令调用者，比如客户点击了按钮，按钮对象执行客户的命令）|Runnable/Thread（其中，Runnable是Command，Runnable的具体实现作为命令，run方法是Receiver，Thread是Invoker，client起到聚合的作用）|
|_1.3 工厂方法（Factory Method）-对象创建型_|单一对象的创建延迟到子类完成|当不知道应该创建哪个产品时，将创建对象任务交给具体子类（知道应该实例化哪个产品）完成|Product（Product抽象）<br>ConcreteProduct（具体的Product）<br>Creator（创建Product的抽象）<br>ConcreteCreator（具体的创建Product的类）<br>Client|java.sql.Driver<br>sun.util.spi.CalendarProvider|
|_1.4 原型（Prototype）-对象创建型_|根据已有对象，复制出新的对象|从头实例化一个对象比较复杂，效率低，通过复制可提高效率|Prototype（抽象出克隆方法的接口）<br>ConcretePrototype（实现克隆方法的具体类）|jdk的Cloneable接口|
|_1.5 单例（Singleton）-对象创建型_|某个类只存在一个实例对象|对开销大的对象只创建一次<br>对需要串行访问的数据或者执行的代码，全局只有一个统一调用入口|Singleton（单例对象）|spring的BeanFactory<br>数据库连接管理器|