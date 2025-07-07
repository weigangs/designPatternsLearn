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
|_2.2 命令（Command）-对象行为型_|将请求者与请求处理者解耦|需要支持撤销、重做<br>支持命令队列<br>菜单按钮点击触发行为<br>比如将文本的复制操作作为一个命令，文本被复制到文档后，我们也可以Ctrl+z撤销复制内容，就是由command对象负责调用对应的文档对象的复制和撤销操作|Command（请求的抽象）<br>ConcreteCommand（具体的请求对象）<br>Receiver（请求的处理对象）<br>Client（客户）<br>Invoker（命令调用者，比如客户点击了按钮，按钮对象执行客户的命令）|Runnable/Thread（其中，Runnable是Command，Runnable的具体实现作为命令，run方法是Receiver，Thread是Invoker，client起到聚合的作用）<br>Spring MVC，IControllerApi(Command), ConcreteController.handler() (ConcreteCommand),DispatchServlet(Invoker), BusiService.handle()或者ConcreteController.handler()代码块(Receiver), 前端发起请求操作（Client）|
|_2.3 解释器（Interpreter）-类行为型_|对某个给定的语言的文法进行解释|自定义的DSL需要解释时，且该语言可以表示为一个抽象语法树时，可以用解释器来进行表达式解释|AbstractExpression（表达式抽象）<br>TerminalExpression（终结符表达式，比如普通字符串或者数字等）<br>NonTerminalExpression（非终结符表达式，比如加号等具有特殊意义的符号）<br>Context(全局共享变量等)<br>Client（解释器使用者）|java.util.regex, 正则表达式(AbstractExpression),//[0-3]{1}//g（Context），\\d（TerminalExpression）, {1}（NonTerminalExpression）<br>Spring Expression Language (SpEL)， Expression（AbstractExpression）， LiteralExpression（TerminalExpression），OpPlus（NonTerminalExpression），StandardEvaluationContext（Context）|
|_2.4 迭代器（Iterator）-对象行为型_|根据给定的顺序遍历一个集合，无需暴露该对象的内部实现|在不暴露聚合对象的内部实现前提下，提供多种遍历方式<br>对不同聚合对象提供统一的遍历接口|Iterator（迭代器抽象）<br>ConcreteIterator（对应集合的迭代器）<br>Aggregate（聚合接口）<br>ConcreteAggregate（具体的聚合类）<br>Client（调用遍历）|java.util.Iterator|
|_2.5 中介者（Mediator）-对象行为型_|简化对象之间的交互关系|对于模块间组织职责划分较为明确，但是各模块间交互又比较复杂的可以使用。<br>比如，用python写了个界面有多个下拉框要联动，每个下拉框改变了都去通知Mediator去通知相关组件刷新，而不是在这个下拉框代码里面引用相关的下拉框代码|Mediator（中介者抽象）<br>ConcreteMediator（具体中介者）<br>ColleagueA（模块A或者功能类A）<br>ColleagueB（模块B或者功能类B）|org.springframework.context.ApplicationEventPublisher<br>其中ApplicationEventPublisher（Mediator）<br>ClassPathXmlApplicationContext(ConcreteMediator)<br>ApplicationListenerA(ColleagueA)<br>ApplicationListenerB(ColleagueB)|
|_2.6 备忘录（Memento）-对象行为型_|对某个对象的内部状态进行快照保存，以便后续恢复等|需要保存某个对象某个时刻的状态，但是不能直接让外部对象访问到这些状态|Caretaker（备忘录保管者）<br>Originator（原发器）<br>Memento（备忘录）|Serializable序列化<br>Serializable(Originator)<br>对象被序列化后的数据（Memento）<br>保存和调用序列化后的代码（Caretaker）|
|_2.7 观察者（Observer）-对象行为型_|当前对象状态改变时，通知依赖的对象及时更新对应的状态|适合发布-订阅场景<br>适合构建事件驱动系统|Subject（目标抽象）<br>ConcreteSubject（具体目标）<br>Observer（观察者抽象）<br>ConcreteObserver（具体观察者）|<br>Observable（Subject）<br>Observer(Observer)<br>ApplicationEventPublisher<br>ApplicationEventPublisher（Subject）<br>Beans with @EventListener （Observer）|
|_2.8 状态（State）-对象行为型_|将对象内部状态与具体行为映射|对象状态改变时，行为也发生变化<br>避免大量if/else<br>将对象状态与对应行为封装起来<br>对象状态与行为关联复杂或者各自独立演变|Context（上下文）<br>State（状态对象以及行为的抽象）<br>ConcreteState(具体状态与行为的子类)|风扇（Context）<br>风扇状态与按钮行为（state），关闭状态与按按钮打开低速动作（CloseState）<br>低速状态与按按钮变高速（LowState）<br>高速状态与按按钮变关闭（HighState）|
|_2.9 策略（Strategy）-对象行为型_|使多个算法独立封装且能互换|当需要的算法/或者行为不止一种时可使用<br>想避免多个if-else or switch时<br>对算法的改变对客户的使用无感知时<br>算法的扩展保持灵活性，且算法是在运行时指定的|Context（上下文）<br>Strategy（算法抽象）<br>ConcreteStrategy(具体算法)|Comparator<br>List（Context）<br>Comparator（Strategy）<br>Comparator.naturalOrder()（ConcreteStrategy）<br>spring中例子<br>DiscountStrategy（Strategy）<br>DiscountService（Context）<br>VipDiscountStrategy（ConcreteStrategy） |
|_2.10 模板方法（Template Method）-类行为型_|定义好算法的不变骨架后，把特定的步骤留给子类实现|流程或者算法的基本骨架是固定的，只是某个步骤不同场景可能有所不同，这时可以将这个步骤变成钩子方法，让子类实现|AbstractClass（流程或者算法骨架在抽象类的方法中定义好）<br>Subclasses（实现了某个特殊步骤的子类）|###Real-world analogy <br> 1. Boil water <br> 2. Brew (tea/coffee) <br> 3. Pour into cup <br> 4. Add condiments (sugar/milk/lemon)<br> This process stays the same, but brewing and condiments differ between coffee and tea. That’s a Template Method!<br>###JdbcTemplate ，RestTemplate，RedisTemplate<br>query(), execute() in JdbcTemplate（Template Method）<br> RowMapper.mapRow()（Custom Step）<br>Connection mgmt, SQL execution（Fixed Steps） |
|_2.11 访问者（Visitor）-对象行为型_|在不改变一组具有相关性对象的前提下，给这些对象增加新的操作|想给结构具有相似性的一组对象增加一些不同的操作<br>将这一组对象与它们的行为分离|Visitor（定义对一组对象的操作的抽象）<br>ConcreteVisitor（作用在这一组对象上的具体的行为）<br> Node(一组对象的抽象)<br>ConcreteNode（具体某个对象）|### javax.lang.model.util.SimpleElementVisitor9<br> ElementVisitor （Visitor）<br>SimpleElementVisitor9(ConcreteVisitor)<br> Element  （Node）<br> TypeElement, ExecutableElement（ConcreteNode）<br>### org.springframework.beans.factory.config.BeanDefinitionVisitor <br> BeanDefinition (Node) <br> BeanDefinitionVisitor （ConcreteVisitor）<br> GenericBeanDefinition(ConcreteNode)  |
## _行为型UML类图_
### 2.1 责任链
![](out\uml\ChainOfResponsibility\ChainOfResponsibility.png)
### 2.2 命令
![](out\uml\Command\Command.png)
### 2.3 解释器
![](out\uml\Interpreter\Interpreter.png)
### 2.4 迭代器
![](out\uml\Iterator\Iterator.png)
### 2.5 中介者
![](out\uml\Mediator\Mediator.png)
### 2.6 备忘录
![](out\uml\Memento\Memento.png)
### 2.7 观察者
![](out\uml\Observer\Observer.png)
### 2.8 状态
![](out\uml\State\State.png)
### 2.9 策略
![](out\uml\Strategy\Strategy.png)
### 2.10 模板方法
![](out\uml\TemplateMethod\TemplateMethod.png)
### 2.11 访问者
![](out\uml\Visitor\Visitor.png)
## 3. 结构型
|模式名称|意图|适用场景|关键参与者|应用实例|
|------|------|------|------|------|
|_3.1 适配器（Adapter）-类对象结构型_|让互不兼容的两个接口，可以一起工作|让已有的库或遗留的接口适配现有代码<br>互不兼容的两个系统间的桥梁<br>系统不同层次的解耦<br>让一个系统能支持不同接口|Target（要使用的目标接口）<br>Client（与Target接口协同工作）<br>Adaptee（已存在的遗留接口）<br>Adapter（对Target和Adaptee进行适配）|### FileInputStream<br> InputStream （Adaptee）<br> InputStreamReader（Adapter）<br> Reader （Target）<br>### SpringMVC HandlerAdapters <br> HandlerAdapter （Adapter）<br> Various controller types （Adaptee）<br> HTTP request handling （Target）<br> ### Spring Security – AuthenticationProvider <br> AuthenticationProvider (Adapter) <br> Your custom auth source (DB, JWT, etc.) （Adaptee）<br> Spring Security authentication flow （Target）|
|_3.2 桥接（Bridge）-对象结构型_|将抽象与实现分离从而让各自独立改变|让接口从框架中独立出来<br>接口抽象的改变与实现的改变独立互不影响<br>在运行时改变实现部分<br>想避免因为要组合太多抽象/实现变体而使类爆炸|Abstraction（抽象类接口）<br>RefinedAbstraction（扩充Abstraction的接口）<br>Implementor（定义实现类的接口）<br>ConcreteImplementor（Implementor的具体实现）|### Spring’s JdbcTemplate and DataSource<br> JdbcTemplate  （abstraction）<br> DataSource （ implementor）<br> HikariDataSource （ConcreteImplementor）<br>###  Spring Cache Abstraction <br> CacheManager  （abstraction）<br> ConcurrentMapCacheManager （RefinedAbstraction）<br> Cache （Implementor）<br>ConcurrentMapCache (ConcreteImplementor) |
|_3.3 组合模式（Composite）-对象结构型_|对整体和部分的操作保持一致|要表示部分与整体的概念<br>对单个对象和组合对象的操作是同一个接口<br>当对象的结构是递归结构时|Component（组合元素的统一抽象）<br>Leaf（组合元素的叶子节点）<br>Composite（定义对多个组合元素的操作对象）<br>Client（通过Component操作组合元素）|### jdk的文件对象FileSystemComponent <br> FileSystemComponent   （Component）<br> File  （Leaf）<br> Directory  （定于对子元素操作的对象）<br>###  java.awt.Container and Component <br> Component  （Component）<br> Container （Composite）<br> Button （Leaf）|
|_3.4 装饰器（Decorator）-对象结构型_|不修改现有对象的前提下动态增加新功能|想在运行时动态的给对象新增职责<br>想简化继承关系<br>对象设计遵循开闭原则<br>想实现灵活的链式行为|Component（元素的抽象）<br>Decorator（装饰器）<br>ConcreteComponent（具体元素对象）<br>ConcreteDecorator（具体的装饰器）|### java.io Streams <br> InputStream   （Component）<br> FileInputStream  （ConcreteComponent）<br> BufferedInputStream  （Decorators）|
|_3.5 外观（Facade）-对象结构型_|为复杂类组成的系统抽象一个简单接口，对外部提供服务|想简化对复杂系统的调用<br> 将客户端与第三方库的内部实现类解耦<br>包装遗留代码，提供一个清晰明了的接口<br>组织管理API层|Facade（子系统接口）<br>Subsystem classes（子系统各功能类）|### java.util.logging.Logger <br> Logger （Facade）<br> log levels, formatters, handlers  （classes）<br> ### JdbcTemplate <br> JdbcTemplate （Facade）<br> support classes （Sub classes）|
|_3.6 享元（Flyweight）-对象结构型_|最大化对象的共享，从而达到内存使用最小|有大量对象共享内部状态<br> 想在高内存容量系统中减少内存使用量<br>想缓存，重新使用不可修改的数据<br>想分离固有状态和外部状态|Flyweight（共享元素抽象）<br>ConcreteFlyweight（具体被共享的元素）<br> FlyweightFactory （管理Flyweight的工厂）|### java.util.logging.Logger <br> Logger （Facade）<br> log levels, formatters, handlers  （classes）<br> ### JdbcTemplate <br> JdbcTemplate （Facade）<br> support classes （Sub classes）|
|_3.7 代理（Proxy）-对象结构型_|控制其它对象对某个对象的访问|保护真是对象（如认证）<br> 延迟加载开销大的对象<br>在真是对象的方法调用前后增加功能<br>用本地对象代表远程资源|Proxy（代理抽象）<br>Subject（具体实体和代理对象的共同接口）<br> RealSubject （具体实体对象）|### java.lang.reflect.Proxy <br> InvocationHandler （Proxy）<br> 具体类  （RealSubject）<br> ### AOP Proxy |
## _结构型UML类图_
### 3.1 适配器
#### 3.1.1 类适配器
![](out\uml\AdapterClass\AdapterClass.png)
#### 3.1.2 对象适配器
![](out\uml\AdapterObject\AdapterObject.png)
### 3.2 桥接
![](out\uml\Bridge\Bridge.png)
### 3.3 组合
![](out\uml\Composite\Composite.png)
### 3.4 装饰器
![](out\uml\Decorator\Decorator.png)
### 3.5 外观
![](out\uml\Facade\Facade.png)
### 3.6 享元
![](out\uml\Flyweight\Flyweight.png)
### 3.7 代理
![](out\uml\Proxy\Proxy.png)