### describe: 描述你的测试
> 一个测试组程序组前面都会开始于一个全局方法：describe，这个方法有两个参数，字符串以及一个函数，字符串用来说明这个程序组的简介，通常用来说明测试的东西是什么。函数是一段用来实现描述的功能的代码块。
### specs:
> 由全局函数it定义，其参数形式与describe一致，一个spec包含了一个或多个测试代码状态的预期值。一个期望在jasmine就是一个断言，可能是true或false.一个所有预期全为true的spec，就称为测试通过的spec，而spec中出现一个或多个false,这个spec就视为测试失败的spec
 ##### 说明：++在describe和it中，我们能书写所有可执行的代码完成测试，在里面的js作用域一致的，所以声明在describe中的变量，也能在该describe块中的所有it函数块中。++

### Expectations: 
> expectations由expect函数生成，expect中的值称为实际值。它后面会跟着链式的匹配器，匹配器中的值为期望值。
> 

### Matchers
> 每个匹配器都实现了一个真实值和期望值的布尔比较，它负责向jasmine报告这个期望是true or false.jasmine根据匹配器的报告，认真这个spec是通过还是失败。

++所有的匹配器都可以通过在expect与matcher之间，加一个not，来获取该匹配器的相反匹配器。++
### Included Matchers
> jasmine有丰富的自带的matcher库，同时提供了[自定义matchers](https://jasmine.github.io/2.5/custom_matcher.html)的能力。

### 通过fail方法 手动设置一个spec为 fail
>   fail()函数手动设置一个spec为失败状态。能接受一个失败信息或error对象作为参数。
  
  ### spec分组
>   describe函数用来将相关的specs进行分组。字符串参数用来命名这个spec集合，并用来与specs的名字进行级联，用来作为spec的全名，这种命名方式使得从一大组测试specs中找到某个spec变得容易。如果命名得好的话，你的specs读起来就像一个完整的形为驱动开发的语句。
  
  ### Setup and Teardown
  
>   为了帮助一个测试套件(suite)处理任何重复的设置和销毁代码，jasmine提供了全局的beforeEach, afterEach，beforeAll,和afterAll方法。
>   顾名思义，beforeEach函数在descirbe中的每一个spec调用前都会被触发，afterEach函数在spec调用后都会被调用。
  
>   这种写法与之前spec的写法有点不同，test中的变量定义在顶层作用域，即describe块中，初始化代码在beforeEach中，afterEach重置变量发生在继续运行下一个spec之前。

> beforeAll方法运行在当前describe中所有的specs都运行之前, afterAll方法在所有的specs都执行完之后运行。这些函数可以用来加速测试套件和昂贵的初始化和销毁。

> 然而，这两个方法在实际应用中要慎用，因为它们不会在spec之间进行重置状态，这很容易不小心在spec之间泄漏状态,使他们错误地通过或失败。

### 关键字This
> 另一个在beforeEach,it 和afterEach中共享变量的方法就是通过this关键字，每个spec中的beforeEach,it和afterEach有一个共同的this空对象，这个对象在下一个spec中的beforeEach,it,afterEach之前，又会被重置为空对象。

### 嵌套的describe块
> describe可以是嵌套的, specs可以定义在任何级别的作用域中，这就使得一个测试套件，可以由一个方法树组成。一个spec执行前，jasmine沿着树按顺序执行每一个beforeEach函数。等所有的spec都执行后，jasmine再类似的执行afterEach
> 

### Disabling Suites
> 程序组(套件),能够通过xdescribe方法进行禁用，这些被禁用的程序组(套件)里面的specs在运行的时候会被跳过，因此，他们的结果会显示为待定(pending)

> 同时，任何没有函数体参数的spec，其结果也会被标记为pending。

> 在spec内部任何时候调用pending方法，不管期望值(expect方法的结果)是多少，这个spec都会被标记为pending，如果往pending方法中传入一个string，会在这个程序组完成时，当作这个spec的结果展示出来。

### Spies
> Jasmine有测试双功能的spies功能，一个spy能存根任何函数并跟踪调用它以及它的所有参数。一个spy只能存在定义它的descrbe或it块中，在spec完成后即会被移除，有特殊的匹配器(matchers)与spy交互。

- toHaveBeenCalled: spy被调用则返回true
- toHaveBeenCalledTimes:如果spy调用了指定的次数，则匹配器将会通过
- toHaveBeenCalledWith: 如果参数列表匹配到任何记录调用的spy,匹配器将会返回true


### Spies - and.callThrough
> 通过spies链式调用callThrough，spy仍将跟踪所有调用，除非将它委托给实际实现(????)

### Spies - and.throwError
 > 通过spies链式调用throwError，所有spy的调用都将throwError的参数作为error抛出
 
### Spies - and.stub
> 可以用stub存根spy任意次数

### Spies -calls
> 所有spy的调用都会被追踪并在calls属性中暴露。
-   calls.any()-如果spy没有被调用过则返回false,只要调用过一次则会返回true
-   calls.count()-返回spy被调用的次数
-   calls.argsFor(index): 返回第index次调用时传入spy的参数
-   calls.allArgs(): 返回所有的调用参数
-   calls.all()：返回所有调用的上下文(this 对象)及参数;
-   calls.mostRecent()：返回最近调用的上下文和参数
-   calls.first()：返回第一次调用的上下文和参数
-   calls.reset()：清除所有spies的跟踪

### Spies:createSpy:
当没有方法去监听的话，可以用jasmine.createSpy创建一个空的spy，这个spy与其它的spy一样，也有跟踪调用，参数等，但是它之后没有具体实现。spies是一个js对象，也能被当作一个js对象使用

#### Spies: createSpyObj
- 同时创建多个spy

#### jasmine.any
jasmine.any接收一个构造函数或类名作为预期值，如果真实值的构建函数与预期的构建函数相同，jasmine.any返回true.

#### jasmine.anything
如果真实值不是null或者undefined的话，jasmine.anything返回true

#### jasmine.objectContaining
用来判断实际值(对象)中是否有预期的特有的key/value键值对

#### jasmine.arrayContaining
用来判断实际值(数组)中是否有预期的特有的数组元素

#### jasmine.stringMathcing
用来匹配某人字符串片断，参数可为正则表达式

#### Custom asymmetric equality tester
当你只需要对比某些标准而不是严格的相当时，你可以定义一个自定义的不对称的相等测试--提供一个对象，这个对象含有asymmetricMatch方法

### Jasmine Clock

Jasmine Clock 给信赖于时间的代码测试提供了可能。
通过在spec或suit中通过jasmine.clock().isntall来安装，并多次使用。
确保你重启原始函数时，要通过jasmine.clock().uninstall()卸载这些