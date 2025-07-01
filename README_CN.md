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