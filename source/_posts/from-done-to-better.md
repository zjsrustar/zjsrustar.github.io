---
title: 从达标到卓越 —— API 设计之道「淘宝FED前端团队」
tags:
  - API设计
categories: Web开发
typora-copy-images-to: from-done-to-better
typora-root-url: from-done-to-better
date: 2017-06-02 21:59:47
---

# 从达标到卓越 —— API 设计之道

作者: 法海 发表于: [2017-02-16](http://taobaofed.org/blog/2017/02/16/a-guide-to-api-design/)

![1496412161324](1496412161324.png)

新技术层出不穷，长江后浪推前浪，而浪潮褪去后能留下来的，是一些经典的设计思想。

在前端界，以前有远近闻名的 jQuery，近来有声名鹊起的 Vue.js。这两者叫好又叫座的原因固然有很多，但是其中有一个共同特质不可忽视，那便是它们的 **API 设计** 非常优雅。

因此这次我想来谈个大课题 —— API 设计之道。

------

## 讨论内容的定义域

本文并不是《jQuery API 赏析》，当我们谈论 API 的设计时，不只局限于讨论「某个框架应该如何设计暴露出来的方法」。作为程序世界分治复杂逻辑的基本协作手段，广义的 API 设计涉及到我们日常开发中的方方面面。

最常见的 API 暴露途径是函数声明（Function Signiture），以及属性字段（Attributes）；而当我们涉及到前后端 IO 时，则需要关注通信接口的数据结构（JSON Schema）；如果还有异步的通信，那么事件（Events）或消息（Message）如何设计也是个问题；甚至，依赖一个包（Package）的时候，包名本身就是接口，你是否也曾碰到过一个奇葩的包名而吐槽半天？

总之，「API 设计」不只关乎到框架或库的设计者，它和每个开发者息息相关。

## 提纲挈领

有一个核心问题是，我们如何评判一个 API 的设计算「好」？在我看来，一言以蔽之，易用。

那「易用」又是什么呢？我的理解是，只要能够足够接近人类的日常语言和思维，并且不需要引发额外的大脑思考，那就是易用。

> Don’t make me think.

具体地，我根据这些年来碰到的大量（反面和正面）案例，归纳出以下这些要点。按照要求从低到高的顺序如下：

- 达标：词法和语法
  - 正确拼写
  - 准确用词
  - 注意单复数
  - 不要搞错词性
  - 处理缩写
  - 用对时态和语态
- 进阶：语义和可用性
  - 单一职责
  - 避免副作用
  - 合理设计函数参数
  - 合理运用函数重载
  - 使返回值可预期
  - 固化术语表
  - 遵循一致的 API 风格
- 卓越：系统性和大局观
  - 版本控制
  - 确保向下兼容
  - 设计扩展机制
  - 控制 API 的抽象级别
  - 收敛 API 集
  - 发散 API 集
  - 制定 API 的支持策略

（本文主要以 JavaScript 作为语言示例。）

## 达标：词法和语法

高级语言和自然语言（英语）其实相差无几，因此正确地使用（英语的）词法和语法是程序员最基本的素养。而涉及到 API 这种供用户调用的代码时，则尤其重要。

但事实上，由于亚洲地区对英语的掌握能力普遍一般……所以现实状况并不乐观 —— 如果以正确使用词法和语法作为达标的门槛，很多 API 都没能达标。

### 正确拼写

正确地拼写一个单词是底线，这一点无需赘述。然而 API 中的各种错别字现象仍屡见不鲜，即使是在我们阿里这样的大公司内。

曾经有某个 JSON 接口（mtop）返回这样一组店铺数据，以在前端模板中渲染：

```
// json
[
  {
    "shopBottom": {
      "isTmall": "false",
      "shopLevel": "916",
      "shopLeveImg": "//xxx.jpg"
    }
  }
]

```

乍一看平淡无奇，结果我调试了小半天都没能渲染出店铺的「店铺等级标识图片」，即`shopLevelImg` 字段。问题到底出在了哪里？

眼细的朋友可能已经发现，接口给的字段名是 `shopLeveImg`，少了一个 `l`，而在其后字母 `I`的光辉照耀下，肉眼很难分辨出这个细节问题。

拼错单词的问题真的是太普遍了，再比如：

- 某个叫做 `toast` 的库，package.json 中的 name 写成了 `taost`。导致在 npm 中没能找到这个包。
- 某个跑马灯组件，工厂方法中的一个属性错将 `panel` 写成了 `pannel`。导致以正确的属性名初始化时代码跑不起来。
- 某个 URL（www.ruanyifeng.com/blog/2017/01/entainment.html）中错将`entertainment` 写成了 `entainment`……这倒没什么大影响，只是 URL 发布后就改不了了，留下了错别字不好看。
- ……

注意到，这些拼写错误经常出现在 **字符串** 的场景中。不同于变量名，IDE 无法检查字符串中的单词是否科学、是否和一些变量名一致，因此，我们在对待一些需要公开出去的 API 时，需要尤其注意这方面的问题；另一方面，更认真地注意 IDE 的 typo 提示（单词拼写错误提示），也会对我们产生很大帮助。

### 准确用词

我们知道，中英文单词的含义并非一一对应，有时一个中文意思可以用不同的英文单词来解释，这时我们需要选择使用恰当的准确的词来描述。

比如中文的「消息」可以翻译为 message、notification、news 等。虽然这几个不同的单词都可以有「消息」的意思，但它们在用法和语境场景上存在着细微差异：

- message：一般指双方通信的消息，是内容载体。而且经常有来有往、成对出现。比如 `postMessage()` 和 `receiveMessage()`。
- notification：经常用于那种比较短小的通知，现在甚至专指 iOS / Android 那样的通知消息。比如 `new NotificationManager()`。
- news：内容较长的新闻消息，比 notification 更重量级。比如 `getTopNews()`。
- feed：自从 RSS 订阅时代出现的一个单词，现在 RSS 已经日薄西山，但是 feed 这个词被用在了更多的地方。其含义只可意会不可言传。比如 `fetchWeitaoFeeds()`。

所以，即使中文意思大体相近，也要准确地用词，从而让读者更易理解 API 的作用和 **上下文场景**。

有一个正面案例，是关于 React 的。（在未使用 ES2015 的）React 中，有两个方法叫做：

```
React.createClass({
  getDefaultProps: function() {
    // return a dictionary
  },
  getInitialState: function() {
    // return a dictionary either
  }
});

```

它们的作用都是用来定义初始化的组件信息，返回值的类型也都一样，但是在方法名上却分别用了 `default` 和 `initial` 来修饰，为什么不统一为一个呢？

原因和 React 的机制有关：

- `props` 是指 Element 的属性，要么是不存在某个属性值后来为它赋值，要么是存在属性的默认值后来将其覆盖。所以这种行为，`default` 是合理的修饰词。
- `state` 是整个 Component 状态机中的某一个特定状态，既然描述为了状态机，那么状态和状态之间是互相切换的关系。所以对于初始状态，用 `initial` 来修饰。

就这么个小小的细节，就可一瞥 React 本身的机制，足以体现 API 设计者的智慧。

另外，最近我还碰到了这样一组事件 API：

```
// event name 1
page.emit('pageShowModal');

// event name 2
page.emit('pageCloseModal');

```

这两个事件显然是一对正反义的动作，在上述案例中，表示「显示窗口」时使用了 `show`，表示「关闭窗口」时使用了 `close`，这都是非常直觉化的直译。而事实上，成对出现的词应该是：`show & hide`、`open & close`。

因此这里必须强调：**成对出现的正反义词不可混用**。在程序世界经常成对出现的词还有：

- in & out
- on & off
- previous & next
- forward & backward
- success & failure
- …

总之，我们可以试着扩充英语的词汇量，使用合适的词，这对我们准确描述 API 有很大的帮助。

### 注意单复数

所有涉及到诸如数组（Array）、集合（Collection）、列表（List）这样的数据结构，在命名时都要使用复数形式：

```
var shopItems = [
  // ...
];

export function getShopItems() {
  // return an array
}

// fail
export function getShopItem() {
  // unless you really return a non-array
}

```

现实往往出人意表地糟糕，前不久刚改一个项目，我就碰到了这样的写法：

```
class MarketFloor extends Component {
  state = {
    item: [
      {}
    ]
  };
}

```

这里的 `item` 实为一个数组，即使它内部只有一个成员。因此应该命名为 `items` 或`itemList`，无论如何，不应该是表示单数的 `item`。

同时要注意，在复数的风格上保持一致，要么所有都是 `-s`，要么所有都是 `-list`。

反过来，我们在涉及到诸如字典（Dictionary）、表（Map）的时候，不要使用复数！

```
// fail
var EVENT_MAPS = {
  MODAL_WILL_SHOW: 'modalWillShow',
  MODAL_WILL_HIDE: 'modalWillHide',
  // ...
};

```

虽然这个数据结构看上去由很多 key-value 对组成，是个类似于集合的存在，但是「map」本身已经包含了这层意思，不需要再用复数去修饰它。

### 不要搞错词性

另外一个容易犯的低级错误是搞错词性，即命名时拎不清名词、动词、形容词……

```
asyncFunc({
  success: function() {},
  fail: function() {}
});

```

`success` 算是一个在程序界出镜率很高的词了，但是有些同学会搞混，把它当做动词来用。在上述案例中，成对出现的单词其词性应该保持一致，这里应该写作 `succeed` 和 `fail`；当然，在这个语境中，最好遵从惯例，使用名词组合 `success` 和 `failure`。

这一对词全部的词性如下：

- n. 名词：success, failure
- v. 动词：succeed, fail
- adj. 形容词：successful, failed（无形容词，以过去分词充当）
- adv. 副词：successfully, fail to do sth.（无副词，以不定式充当）

注意到，如果有些词没有对应的词性，则考虑变通地采用其他形式来达到同样的意思。

所以，即使我们大部分人都知道：方法命名用动词、属性命名用名词、布尔值类型用形容词（或等价的表语），但由于对某些单词的词性不熟悉，也会导致最终的 API 命名有问题，这样的话就很尴尬了。

### 处理缩写

关于词法最后一个要注意的点是缩写。有时我们经常会纠结，首字母缩写词（acronym）如 `DOM`、`SQL` 是用大写还是小写，还是仅首字母大写，在驼峰格式中又该怎么办……

对于这个问题，简单不易混淆的做法是，首字母缩写词的所有字母均大写。（如果某个语言环境有明确的业界惯例，则遵循惯例。）

```
// before
export function getDomNode() {}

// after
export function getDOMNode() {}

```

在经典前端库 KISSY 的早期版本中，`DOM` 在 API 中都命名为 `dom`，驼峰下变为 `Dom`；而在后面的版本内统一写定为全大写的 `DOM`。

另外一种缩写的情况是对长单词简写（shortened word），如 `btn (button)`、`chk (checkbox)`、`tpl (template)`。这要视具体的语言规范 / 开发框架规范而定。如果什么都没定，也没业界惯例，那么把单词写全了总是不会错的。

### 用对时态和语态

由于我们在调用 API 时一般类似于「调用一条指令」，所以在语法上，一个函数命名是祈使句式，时态使用一般现在时。

但在某些情况下，我们需要使用其他时态（进行时、过去时、将来时）。比如，当我们涉及到 **生命周期**、**事件节点**。

在一些组件系统中，必然涉及到生命周期，我们来看一下 React 的 API 是怎么设计的：

```
export function componentWillMount() {}
export function componentDidMount() {}
export function componentWillUpdate() {}
export function componentDidUpdate() {}
export function componentWillUnmount() {}

```

React 划分了几个关键的生命周期节点（mount, update, unmount, …），以将来时和过去时描述这些节点片段，暴露 API。注意到一个小细节，React 采用了 `componentDidMount`这种过去时风格，而没有使用 `componentMounted`，从而跟 `componentWillMount` 形成对照组，方便记忆。

同样地，当我们设计事件 API 时，也要考虑使用合适的时态，特别是希望提供精细的事件切面时。或者，引入 `before`、`after` 这样的介词来简化：

```
// will render
Component.on('beforeRender', function() {});

// now rendering
Component.on('rendering', function() {});

// has rendered
Component.on('afterRender', function() {});

```

另一方面是关于语态，即选用主动语态和被动语态的问题。其实最好的原则就是 **尽量避免使用被动语态**。因为被动语态看起来会比较绕，不够直观，因此我们要将被动语态的 API 转换为主动语态。

写成代码即形如：

```
// passive voice, make me confused
object.beDoneSomethingBy(subject);

// active voice, much more clear now
subject.doSomething(object);

```

## 进阶：语义和可用性

说了那么多词法和语法的注意点，不过才是达标级别而已。确保 API 的可用性和语义才使 API 真正「可用」。

无论是友好的参数设置，还是让人甜蜜蜜的语法糖，都体现了程序员的人文关怀。

### 单一职责

单一职责是软件工程中一条著名的原则，然而知易行难，一是我们对于具体业务逻辑中「职责」的划分可能存在难度，二是部分同学仍没有养成贯彻此原则的习惯。

小到函数级别的 API，大到整个包，保持单一核心的职责都是很重要的一件事。

```
// fail
component.fetchDataAndRender(url, template);

// good
var data = component.fetchData(url);
component.render(data, template);

```

如上，将混杂在一个大坨函数中的两件独立事情拆分出去，保证函数（function）级别的职责单一。

更进一步地，（假设）`fetchData` 本身更适合用另一个类（class）来封装，则对原来的组件类 `Component` 再进行拆分，将不属于它的取数据职责也分离出去：

```
class DataManager {
  fetchData(url) {}
}

class Component {
  constructor() {
    this.dataManager = new DataManager();
  }
  render(data, template) {}
}

// more code, less responsibility
var data = component.dataManager.fetchData(url);
component.render(data, template);

```

在文件（file）层面同样如此，一个文件只编写一个类，保证文件的职责单一（当然这对很多语言来说是天然的规则）。

最后，视具体的业务关联度而决定，是否将一簇文件做成一个包（package），或是拆成多个。

### 避免副作用

严格「无 [副作用](https://en.wikipedia.org/wiki/Side_effect_(computer_science)) 的编程」几乎只出现在纯函数式程序中，现实中的 OOP 编程场景难免触及副作用。因此在这里所说的「避免副作用」主要指的是：

1. 函数本身的运行稳定可预期。
2. 函数的运行不对外部环境造成意料外的污染。

对于无副作用的纯函数而言，输入同样的参数，执行后总能得到同样的结果，这种幂等性使得一个函数无论在什么上下文中运行、运行多少次，最后的结果总是可预期的 —— 这让用户非常放心，不用关心函数逻辑的细节、考虑是否应该在某个特定的时机调用、记录调用的次数等等。希望我们以后设计的 API 不会出现这个案例中的情况：

```
// return x.x.x.1 while call it once
this.context.getSPM();

// return x.x.x.2 while call it twice
this.context.getSPM();

```

在这里，`getSPM()` 用来获取每个链接唯一的 SPM 码（SPM 是阿里通用的埋点统计方案）。但是用法却显得诡异：每调用一次，就会返回一个不同的 SPM 串，于是当我们需要获得几个 SPM 时，就会这样写：

```
var spm1 = this.context.getSPM();
var spm2 = this.context.getSPM();
var spm3 = this.context.getSPM();

```

虽然在实现上可以理解 —— 此函数内部维护了一个计数器，每次返回一个自增的 SPM D 位，但是 **这样的实现方式与这个命名看似是幂等的 getter 型函数完全不匹配**，换句话说，这使得这个 API 不可预期。

如何修改之？一种做法是，不改变此函数内部的实现，而是将 API 改为 Generator 式的风格，通过形如 `SPMGenerator.next()` 接口来获取自增的 SPM 码。

另一种做法是，如果要保留原名称，可以将函数签名改为 `getSPM(spmD)`，接受一个自定义的 SPM D 位，然后返回整个 SPM 码。这样在调用时也会更明确。

除了函数内部的运行需可预期外，它对外部一旦造成不可预期的污染，那么影响将更大，而且更隐蔽。

对外部造成污染一般是两种途径：一是在函数体内部直接修改外部作用域的变量，甚至全局变量；二是通过修改实参间接影响到外部环境，如果实参是引用类型的数据结构。

曾经也有发生因为对全局变量操作而导致整个容器垮掉的情况，这里就不再展开。

如何防止此类副作用发生？本质上说，需要控制读写权限。比如：

1. 模块沙箱机制，严格限定模块对外部作用域的修改；
2. 对关键成员作访问控制（access control），冻结写权限等等。

### 合理设计函数参数

对一个函数来说，「函数签名」（Function Signature）比函数体本身更重要。函数名、参数设置、返回值类型，这三要素构成了完整的函数签名。而其中，参数设置对用户来说是接触最频繁，也最为关心的部分。

那如何优雅地设计函数的入口参数呢？我的理解是这样几个要点：

优化参数顺序。**相关性越高的参数越要前置**。

这很好理解，相关性越高的参数越重要，越要在前面出现。其实这还有两个隐含的意思，即**可省略的参数后置**，以及 **为可省略的参数设定缺省值**。对某些语言来说（如 C++），调用的时候如果想省略实参，那么一定要为它定义缺省值，而带缺省值的参数必须后置，这是在编译层面就规定死的。而对另一部分灵活的语言来说（如 JS），将可省参数后置同样是最佳实践。

```
// bad
function renderPage(pageIndex, pageData) {}

renderPage(0, {});
renderPage(1, {});

// good
function renderPage(pageData, pageIndex = 0) {}

renderPage({});
renderPage({}, 1);

```

第二个要点是控制参数个数。用户记不住过多的入口参数，因此，参数能省略则省略，或更进一步，**合并同类型的参数**。

由于可以方便地创建 Object 这种复合数据结构，合并参数的这种做法在 JS 中尤为普遍。常见的情况是将很多配置项都包成一个配置对象：

```
// traditional
$.ajax(url, params, success);

// or
$.ajax({
  url,
  params,
  success,
  failure
});

```

这样做的好处是：

- 用户虽然仍需记住参数名，但不用再关心参数顺序。
- 不必担心参数列表过长。将参数合并为字典这种结构后，想增加多少参数都可以，也不用关心需要将哪些可省略的参数后置的问题。

当然，凡事有利有弊，由于缺乏顺序，就无法突出哪些是最核心的参数信息；另外，在设定参数的默认值上，会比参数列表的形式更繁琐。因此，需要兼顾地使用最优的办法来设计函数参数，为了同一个目的：易用。

### 合理运用函数重载

谈到 API 的设计，尤其是函数的设计，总离不开一个机制：重载（overload）。

对于强类型语言来说，重载是个很 cool 的功能，能够大幅减少函数名的数量，避免命名空间的污染。然而对于弱类型语言而言，由于不需要在编译时做 type-binding，函数在调用阶段想怎么传实参都行……所以重载在这里变得非常微妙。以下着重谈一下，什么时候该选择重载，什么时候又不该。

```
Element getElementById(String: id)

HTMLCollection getElementsByClassName(String: names)

HTMLCollection getElementsByTagName(String: name)

```

以上三个函数是再经典不过的 DOM API，而在当初学习它们的时候（从 Java 思维转到 JS 思维）我就在想这两个问题：

1. 为什么要设计成 `getSomethingBySomething` 这么复杂结构的名字，而不是使用`getSomething` 做重载？
2. 这三个函数只有 `getElementById` 是单数形式，为何不设计为返回 HTMLCollection（即使只返回一个成员也可以包一个 Collection 嘛），以做成复数形式的函数名从而保持一致性？

两个问题中，如果第二个问题能解决，那么这三个函数的结构将完全一致，从而可以考虑解决第一个问题。

先来看问题二。稍微深入下 DOM 知识后就知道，id 对于整个 DOM 来说必须是唯一的，因此在理论上 `getElementsById`（注意有复数）将永远返回仅有 0 或 1 个成员的 Collection，这样一来用户的调用方式将始终是 `var element = getElementsById(id)[0]`，而这是非常荒谬的。所以 DOM API 设计得没问题。

既然问题二无解，那么自然这三个函数没法做成一个重载。退一步说，即使问题二能解决，还存在另外一个麻烦：它们的入口参数都是一样的，都是 String！对于强类型语言来说，参数类型和顺序、返回值统统一样的情况下，压根无法重载。因为编译器无法通过任何一个有效的特征，来执行不同的逻辑！

所以，**如果入口参数无法进行有效区分，不要选择重载**。

当然，有一种奇怪的做法可以绕过去：

```
// fail
function getElementsBy(byWhat, name) {
  switch(byWhat) {
    case 'className':
      // ...
    case 'tagName':
      // ...
  }
}

getElementsBy('tagName', name);
getElementsBy('className', name);

```

一种在风格上类似重载的，但实际是在运行时走分支逻辑的做法……可以看到，API 的信息总量并没降低。不过话不能说死，这种风格在某些特定场景也有用武之地，只是多数情况下并不推荐。

与上述风格类似的，是这样一种做法：

```
// get elements by tag-name by default
HTMLCollection getElements(String: name)

// if you add a flag, it goes by class-name
HTMLCollection getElements(String: name, Boolean: byClassName)

```

「将 flag 标记位作为了重载手段」—— 在早期微软的一些 API 中经常能见到这样的写法，可以说一旦离开了文档就无法编码，根本不明白某个 Boolean 标记位是用来干嘛的，这大大降低了用户的开发体验，以及代码可读性。

这样看起来，可重载的场景真是太少了！也不尽然，在我看来有一种场景很适合用重载：批量处理。

```
Module handleModules(Module: module)

Collection<Module> handleModules(Collection<Module>: modules)

```

当用户经常面临处理一个或多个不确定数量的对象时，他可能需要思考和判断，什么时候用单数 `handleModule`、什么时候用复数 `handleModules`。将这种类型的操作重载为一个（大抵见于 setter 型操作），同时支持单个和批量的处理，可以降低用户的认知负担。

所以，在合适的时机重载，否则宁愿选择「函数名结构相同的多个函数」。原则是一样的，保证逻辑正确的前提下，尽可能降低用户负担。

对了，关于 `getElements` 那三个 API，它们最终的进化版本回到了同一个函数：`querySelector(selectors)`。

### 使返回值可预期

函数的易用性体现在两方面：入口和出口。上面已经讲述了足够多关于入口的设计事项，这一节讲出口：函数返回值。

对于 getter 型的函数来说，调用的直接目的就是为了获得返回值。因此我们要让返回值的类型和函数名的期望保持一致。

```
// expect 'a.b.c.d'
function getSPMInString() {

  // fail
  return {
    a, b, c, d
  };
}

```

从这一点上来讲，要慎用 ES2015 中的新特性「解构赋值」。

而对于 setter 型的函数，调用的期望是它能执行一系列的指令，然后去达到一些副作用，比如存文件、改写变量值等等。因此绝大多数情况我们都选择了返回 undefined / void —— 这并不总是最好的选择。

回想一下，我们在调用操作系统的命令时，系统总会返回「exit code」，这让我们能够获知系统命令的执行结果如何，而不必通过其他手段去验证「这个操作到底生效了没」。因此，创建这样一种返回值风格，或可一定程度增加健壮性。

另外一个选项，是让 setter 型 API 始终返回 `this`。这是 jQuery 为我们带来的经典启示 —— 通过返回 `this`，来产生一种「链式调用（chaining）」的风格，简化代码并且增加可读性：

```
$('div')
  .attr('foo', 'bar')
  .data('hello', 'world')
  .on('click', function() {});

```

最后还有一个异类，就是异步执行的函数。由于异步的特性，对于这种需要一定延时才能得到的返回值，只能使用 callback 来继续操作。使用 Promise 来包装它们尤为必要。对异步操作都返回一个 Promise，使整体的 API 风格更可预期。

### 固化术语表

在前面的词法部分中曾经提到「准确用词」，但即使我们已经尽量去用恰当的词，在有些情况下仍然不免碰到一些难以抉择的尴尬场景。

比如，我们经常会看到 pic 和 image、path 和 url 混用的情况，这两组词的意思非常接近（当然严格来说 path 和 url 的意义是明确不同的，在此暂且忽略），稍不留神就会产生 4 种组合……

- picUrl
- picPath
- imageUrl
- imagePath
- 更糟糕的情况是 imgUrl、picUri、picURL……

所以，在一开始就要 **产出术语表**，包括对缩写词的大小写如何处理、是否有自定义的缩写词等等。一个术语表可以形如：

| 标准术语   | 含义   | 禁用的非标准词                     |
| ------ | ---- | --------------------------- |
| pic    | 图片   | image, picture              |
| path   | 路径   | URL, url, uri               |
| on     | 绑定事件 | bind, addEventListener      |
| off    | 解绑事件 | unbind, removeEventListener |
| emit   | 触发事件 | fire, trigger               |
| module | 模块   | mod                         |

不仅在公开的 API 中要遵守术语表规范，在局部变量甚至字符串中都最好按照术语表来。

```
page.emit('pageRenderRow', {
  index: this.props.index,
  modList: moduleList
});

```

比如这个我最近碰到的案例，同时写作了 `modList` 和 `moduleList`，这就有点怪怪的。

另外，对于一些创造出来的、业务特色的词汇，如果不能用英语简明地翻译，就直接用拼音：

- 淘宝 `Taobao`
- 微淘 `Weitao`
- 极有家 `Jiyoujia`
- ……

在这里，千万不要把「微淘」翻译为 `MicroTaobao`……当然，专有词已经有英文名的除外，如 `Tmall`。

### 遵循一致的 API 风格

这一节算得上是一个复习章节。词法、语法、语义中的很多节都指向同一个要点：一致性。

> 一致性可以最大程度降低信息熵。

好吧，这句话不是什么名人名言，就是我现编的。总而言之，一致性能大大降低用户的学习成本，并对 API 产生准确的预期。

1. 在词法上，提炼术语表，全局保持一致的用词，避免出现不同的但是含义相近的词。
2. 在语法上，遵循统一的语法结构（主谓宾顺序、主被动语态），避免天马行空的造句。
3. 在语义上，合理运用函数的重载，提供可预期的甚至一致类型的函数入口和出口。

甚至还可以一致得更细节些，只是举些例子：

1. 打 log 要么都用中文，要么都用英文。
2. 异步接口要么都用回调，要么都改成 Promise。
3. 事件机制只能选择其一：`object.onDoSomething = func` 或 `object.on('doSomething', func)`。
4. 所有的 setter 操作必须返回 `this`。
5. ……

> 一份代码写得再怎么烂，把某个单词都拼成一样的错误，也好过这个单词只出现一次错误。

是的，一致性，再怎么强调都不为过。

## 卓越：系统性和大局观

不管是大到发布至业界，或小到在公司内跨部门使用，一组 API 一旦公开，整体上就是一个产品，而调用方就是用户。所谓牵一发而动全身，一个小细节可能影响整个产品的面貌，一个小改动也可能引发整个产品崩坏。因此，我们一定要站在全局的层面，甚至考虑整个技术环境，系统性地把握整个体系内 API 的设计，体现大局观。

### 版本控制

80% 的项目开发在版本控制方面做得都很糟糕：随心所欲的版本命名、空洞诡异的提交信息、毫无规划的功能更新……人们显然需要一段时间来培养规范化开发的风度，但是至少得先保证一件事情：

> 在大版本号不变的情况下，API 保证向前兼容。

这里说的「大版本号」即「语义化版本命名」`<major>.<minor>.<patch>` 中的第一位 `<major>`位。

这一位的改动表明 API 整体有大的改动，很可能不兼容，因此用户对大版本的依赖改动会慎之又慎；反之，如果 API 有不兼容的改动，意味着必须修改大版本号，否则用户很容易出现在例行更新依赖后整个系统跑不起来的情况，更糟糕的情况则是引发线上故障。

如果这种情况得不到改善，用户们就会选择 **永远不升级依赖**，导致更多的潜在问题。久而久之，最终他们便会弃用这些产品（库、中间件、whatever）。

所以，希望 API 的提供者们以后不会再将大版本锁定为 `0`。更多关于「语义化版本」的内容，请参考我的另一篇文章《[论版本号的正确打开方式](http://taobaofed.org/blog/2016/08/04/instructions-of-semver/)》。

### 确保向下兼容

如果不希望对客户造成更新升级方面的困扰，我们首先要做好的就是确保 API 向下兼容。

API 发生改动，要么是需要提供新的功能，要么是为之前的糟糕设计买单……具体来说，改动无外乎：增加、删除、修改 三方面。

首先是删除。**不要轻易删除公开发布的 API**，无论之前写得多么糟糕。如果一定要删除，那么确保正确使用了「`Deprecated`」：

对于某个不想保留的可怜 API，先不要直接删除，将其标记为 `@deprecated` 后置入下一个小版本升级（比如从 `1.0.2` 到 `1.1.0`）。

```
/**
* @deprecated
*/
export function youWantToRemove(foo, bar) {}

/**
* This is the replacement.
*/
export function youWantToKeep(foo) {}

```

并且，在 changelog 中明确指出这些 API 即将移除（不推荐使用，但是目前仍然能用）。

之后，在下一个 **大版本** 中（比如 `1.1.0` 到 `2.0.0`）删除标记为 `@deprecated` 的部分，同时在 changelog 中指明它们已删除。

其次是 API 的修改。如果我们仅仅是修复 bug、重构实现、或者添加一些小特性，那自然没什么可说的；但是如果想彻底修改一个 API……比如重做入口参数、改写业务逻辑等等，建议的做法是：

1. 确保原来的 API 符合「单一职责」原则，如果不是则修改之。
2. 增加一个全新的 API 去实现新的需求！由于我们的 API 都遵循「单一职责」，因此一旦需要彻底修改 API，意味着新需求和原来的职责已经完全无法匹配，不如干脆新增一个 API。
3. 视具体情况选择保留或移除旧 API，进入前面所述「删除 API」的流程。

最后是新增 API。事实上，即使是只加代码不删代码，整体也不一定是向下兼容的。有一个经典的正面案例是：

```
// modern browsers
document.hidden == false;

// out-of-date browsers
document.hidden == undefined;

```

浏览器新增的一个 API，用以标记「当前文档是否可见」。直观的设计应该是新增`document.visible` 这样的属性名……问题是，在逻辑上，文档默认是可见的，即`document.visible` 默认为 `true`，而不支持此新属性的旧浏览器返回 `document.visible == undefined`，是个 falsy 值。因此，如果用户在代码中简单地以：

```
if (document.visible) {
  // do some stuff
}

```

做特征检测的话，在旧浏览器中就会进入错误的条件分支……而反之，以 `document.hidden`API 来判断，则是向下兼容的。

### 设计扩展机制

毫无疑问，在保证向下兼容的同时，API 需要有一个对应的扩展机制以可持续发展 —— 一方面便于开发者自身增加功能，另一方面用户也能参与进来共建生态。

技术上来说，接口的扩展方式有很多，比如：继承（extend）、组合（mixin）、装饰（decorate）……选择没有对错，因为不同的扩展方式适用于不同的场景：在逻辑上确实存在派生关系，并且需要沿用基类行为同时自定义行为的，采用重量级的继承；仅仅是扩充一些行为功能，但是逻辑上压根不存在父子关系的，使用组合；而装饰手法更多应用于给定一个接口，将其包装成多种适用于不同场景新接口的情况……

另一方面，对于不同的编程语言来说，由于不同的语言特性……静态、动态等，各自更适合用某几种扩展方式。所以，到底采用什么扩展办法，还是得视情况而定。

在 JS 界，有一些经典的技术产品，它们的扩展甚至已经形成生态，如：

- jQuery。耳熟能详的 `$.fn.customMethod = function() {};`。这种简单的 mixin 做法已经为 jQuery 提供了成千上万的插件，而 jQuery 自己的大部分 API 本身也是基于这个写法构建起来的。
- React。React 自身已经处理了所有有关组件实例化、生命周期、渲染和更新等繁琐的事项，只要开发者基于 `React.Component` 来继承出一个组件类。对于一个 component system 来说，这是一个经典的做法。
- Gulp。相比于近两年的大热 Webpack，个人认为 Gulp 更能体现一个 building system 的逻辑 —— 定义各种各样的「任务」，然后用「管道」将它们串起来。一个 Gulp 插件也是那么的纯粹，接受文件流，返回文件流，如是而已。
- Koa。对于主流的 HTTP Server 来说，中间件的设计大同小异：接受上一个 request，返回一个新的 response。而对天生 Promise 化的 Koa 来说，它的中间件风格更接近于 Gulp 了，区别仅在于一个是 file stream，一个是 HTTP stream。

不只是庞大的框架需要考虑扩展性，设计可扩展的 API 应该变成一种基本的思维方式。比如这个活生生的业务例子：

```
// json
[
  {
    "type": "item",
    "otherAttrs": "foo"
  },
  {
    "type": "shop",
    "otherAttrs": "bar"
  }
]

// render logic
switch(feed.type) {
  case 'item':
    console.log('render in item-style.');
    break;
  case 'shop':
    console.log('render in shop-style.');
    break;
  case 'other':
  default:
    console.log('render in other styles, maybe banner or sth.');
    break;
}

```

根据不同的类型渲染一组 feeds 信息：商品模块、店铺模块，或是其他。某天新增了需求说要支持渲染天猫的店铺模块（多显示个天猫标等等），于是 JSON 接口直接新增一个`type = 'tmallShop'` —— 这种接口改法很简单直观，但是并不好。在不改前端代码的情况下，`tmallShop` 类型默认进入 `default` 分支，导致奇奇怪怪的渲染结果。

考虑到 `tmallShop` 和 `shop` 之间是一个继承的关系，`tmallShop` 完全可以当一个普通的 `shop`来用，执行后者的所有逻辑。用 Java 的表达方式来说就是：

```
// a tmallShop is a shop
Shop tmallShop = new TmallShop();
tmallShop.doSomeShopStuff();

```

将这个逻辑关系反映到 JSON 接口中，合理的做法是新增一个 `subType` 字段，用来标记`tmallShop`，而它的 `type` 仍然保持为 `shop`。这样一来，即使原来的前端代码完全不修改，仍然可以正常运行，除了无法渲染出一些天猫店铺的特征。

这里还有一个非常类似的正面案例，是 ABS 搭建系统（淘宝 FED 出品的站点搭建系统）设计的模块 JSON Schema：

```
// json
[
  {
    "type": "string",
    "format": "enum"
  }, {
    "type": "string",
    "format": "URL"
  }
]

```

同样采用了 `type` 为主类型，而扩展字段在这里变成了 `format`，用来容纳一些扩展特性。在实际开发中，的确也很方便新增各种新的数据结构逻辑。

### 控制 API 的抽象级别

API 能扩展的前提是什么？是接口足够抽象。这样才能够加上各种具体的定语、装饰更多功能。用日常语言举个例子：

```
// abstract
I want to go to a place.
// when
{Today, Tomorrow, Jan. 1st} I want to go to a place.
// where
I want to go to {mall, cafe, bed}.

// concrete, no extends any more
Today I want to go to a cafe for my business.

```

所以，在设计 API 时要高抽象，不要陷入具体的实现，不要陷入具体的需求，要高屋建瓴。

看个实际的案例：一个类 React Native 的页面框架想暴露出一个事件「滚动到第二屏」，以便页面开发者能监听这个事件，从而更好地控制页面资源的加载策略（比如首屏默认加载渲染、到第二屏之后再去加载剩下的资源）。

但是因为一些实现上的原因，页面框架还不能通过页面位移（offset）来精确地通知「滚动到了第二屏」，而只能判断「第二屏的第一个模块出现了」。于是这个事件没有被设计为`secondScreenReached`，而变成了 `secondScreenFirstModuleAppear`……虽然`secondScreenFirstModuleAppear` 不能精确定义 `secondScreenReached`，但是直接暴露这个具体的 API 实在太糟糕了，问题在于：

- 用户在依赖一个非常非常具体的 API，给用户造成了额外的信息负担。「第二屏的第一个模块出现了！」这很怪异，用户根本不关心模块的事情，用户关心的只是他是否到达了第二屏。
- 一旦页面框架能够真正通过页面位移来实现「滚动到第二屏」，如果我们暴露的是高抽象的 `secondScreenReached`，那么只需要更改一下这个接口的具体实现即可；反之，我们暴露的是很具体的 `secondScreenFirstModuleAppear`，就只能挨个通知用户：「你现在可以不用依赖这个事件了，改成我们新出的 `secondScreenReached` 吧！」

是的，抽象级别一般来说越高越好，将 API 设计成业务无关的，更通用，而且方便扩展。但是物极必反，对于像我这样的抽象控来说，最好能学会控制接口的抽象级别，将其保持在一个恰到好处的层次上，不要做无休止的抽象。

还是刚才的例子 `secondScreenReached`，我们还可以将其抽象成 `targetScreenReached`，可以支持到达首屏、到达第二屏、第三屏……的事件，这样是不是更灵活、更优雅呢？并没有 ——

- 抽象时一定要考虑到具体的业务需求场景，有些实现路径如果永远不可能走到，就没必要抽出来。比如这个例子中，没有人会去关心第三屏、第四屏的事件。
- 太高的抽象容易造成太多的层次，带来额外的耦合、通信等不同层次之间的沟通成本，这将会成为新的麻烦。对用户而言，也是额外的信息负担。

对于特定的业务来说，接口越抽象越通用，而越具体则越能解决特定问题。所以，思考清楚，API 面向的场景范围，避免懒惰设计，避免过度设计。

### 收敛 API 集

对于一整个体系的 API 来说，用户面对的是这个整体集合，而不是其中某几个单一的 API。我们要保证集合内的 API 都在一致的抽象维度上，并且适当地合并 API，减小整个集合的信息量，酌情做减法。

> 产品开始做减法，便是对用户的温柔。

**收敛近似意义的参数和局部变量**。下面这样的一组 API 好像没什么不对，但是对强迫症来说一定产生了不祥的直觉：

```
export function selectTab(index) {}

export function highlightTab(tabIndex) {}

export function gotoPage(index) {}

```

又是 `index` 又是 `tabIndex` 的，或许还会有 `pageIndex`？诚然，函数形参和局部变量的命名对最终用户来说没有直接影响，但是这些不一致的写法仍然能反映到 API 文档中，并且，对开发者自身也会产生混淆。所以，选一个固定的命名风格，然后从一而终！如果忘了的话，回头看一下前文「固化术语表」这一节吧！

**收敛近似职责的函数**。对用户暴露出太多的接口不是好事，但是一旦要合并不同的函数，是否就会破坏「单一职责」原则呢？

不，因为「单一职责」本身也要看具体的抽象层次。以下这个例子和前文「合理运用函数重载」中的例子有相似之处，但具体又有所不同。

```
// a complex rendering process
function renderPage() {

  // too many APIs here
  renderHeader();
  renderBody();
  renderSidebar();
  renderFooter();
}

// now merged
function renderPage() {
  renderSections([
    'header', 'body', 'sidebar', 'footer'
  ]);
}

// call renderSection
function renderSections(sections) {}

// and the real labor
function renderSection(section) {}

```

类似于这样，避免暴露过多近似的 API，合理利用抽象将其合并，减小对用户的压力。

对于一个有清晰继承树的场景来说，收敛 API 显得更加自然且意义重大 —— 利用多态性（Polymorphism）构建 Consistent APIs。（以下例子来源于 [Clean Code JS](https://github.com/ryanmcdermott/clean-code-javascript)。）

```
// bad: type-checking here
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}

// cool
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}

```

有一个将 API 收敛到极致的家伙恐怕大家都不会陌生：jQuery 的 `$()`。这个风格不正是 jQuery 当年的杀手级特性之一吗？

> 如果 `$()` 能让我搞定这件事，就不要再给我 `foo()` 和 `bar()`。

**收敛近似功能的包**。再往上一级，我们甚至可以合并相近的 package。

淘宝 FED 的 Rax 体系（类 RN 框架）中，有基础的组件标签，如 `<Image> (in @ali/rax-components)`、`<Link> (in @ali/rax-components)`，也有一些增强功能的 package，如`<Picture> (in @ali/rax-picture)`、`<Link> (in @ali/rax-spmlink)`。

在这里，后者包之于前者相当于装饰了更多功能，是前者的增强版。而在实际应用中，也是推荐使用诸如 `<Picture>` 而禁止使用 `<Image>`。那么在这种大环境下，`<Image>` 等基础 API 的暴露就反而变得很扰民。可以考虑将增强包的功能完全合并入基础组件，即将 `<Picture>`并入 `<Image>`，用户只需面对单一的、标准的组件 API。

### 发散 API 集

这听上去很荒谬，为什么一个 API 集合又要收敛又要发散？仅仅是为了大纲上的对称性吗？

当然不是。存在这个小节是因为我有一个不得不提的案例，不适合放在其他段落，只能放在这里……不，言归正传，我们有时的确需要发散 API 集，提供几个看似接近的 API，以引导用户。因为 —— 虽然这听起来很荒谬 —— 某些情况下，API 其实不够用，但是用户 **没有意识到 API 不够用**，而是选择了混用、滥用。看下面这个例子：

```
// the func is used here
requestAnimationFrame(() => {

  // what? trigger an event?
  emitter.emit('moduleDidRenderRow');
});

// ...and there
requestAnimationFrame(() => {

  // another one here, I guess rendering?
  this.setState({
    // ...
  });
});

```

在重构一组代码时，我看到代码里充斥着 `requestAnimationFrame()`，这是一个比较新的全局 API，它会以接近 60 FPS 的速率延时执行一个传入的函数，类似于一个针对特定场景优化过的 `setTimeout()`，但它的初衷是用来绘制动画帧的，而不应该用在奇奇怪怪的场景中。

在深入地了解了代码逻辑之后，我认识到这里如此调用是为了「延时一丢丢执行一些操作」，避免阻塞主渲染线程。然而这种情况下，还不如直接调用 `setTimeout()` 来做延时操作。虽然没有太明确的语义，但是至少好过把自己伪装成一次动画的绘制。更可怕的是，据我所知 `requestAnimationFrame()` 的滥用不仅出现在这次重构的代码中，我至少在三个不同的库见过它的身影 —— 无一例外地，这些库和动画并没有什么关系。

（一个可能的推断是，调用 `requestAnimationFrame(callback)` 时不用指定 `timeout` 毫秒数，而 `setTimeout(callback, timeout)` 是需要的。似乎对很多用户来说，前者的调用方式更 cool？）

所以，在市面上有一些 API 好像是「偏方」一般的存在：虽然不知道为什么要这么用，但是……用它就对了！

事实上，对于上面这个场景，最恰当的解法是使用一个更加新的 API，叫做`requestIdleCallback(callback)`。这个 API 从名字上看起来就很有语义：在线程空闲的时候再执行操作。这完全契合上述场景的需求，而且还自带底层的优化。

当然，由于 API 比较新，还不是所有的平台都能支持。即便如此，我们也可以先面向接口编程，自己做一个 polyfill：

```
// simple polyfill
export function requestIdleCallback(callback) => {
  callback && setTimeout(callback, 1e3 / 60);
};

```

另一个经典的滥用例子是 ES2015 中的「Generator / yield」。

原本使用场景非常有限的生成器 Generator 机制被大神匠心独运地加以改造，包装成用来异步代码同步化的解决方案。这种做法自然很有创意，但是从语义用法上来说实在不足称道，让代码变得非常难读，并且带来维护隐患。与其如此，还不如仅仅使用 Promise。

令人欣慰的是，随后新版的 ES 即提出了新的异步代码关键字「async / await」，真正在语法层面解决了异步代码同步化的问题，并且，新版的 Node.js 也已经支持这种语法。

因此，我们作为 API 的开发者，一定要提供足够场景适用的 API，来引导我们的用户，不要让他们做出一些出人意料的「妙用」之举。

### 制定 API 的支持策略

我们说，一组公开的 API 是产品。而产品，一定有特定的用户群，或是全球的开发者，或仅仅是跨部门的同事；产品同时有保质期，或者说，生命周期。

面向目标用户群体，我们要制定 API 的支持策略：

- 每一个大版本的支持周期是多久。
- 是否有长期稳定的 API 支持版本。（Long-term Support）
- 如何从旧版本升级。

老旧版本很可能还在运行，但维护者已经没时间精力再去管这些历史遗物，这时明确地指出某些版本不再维护，对开发者和用户都好。当然，同时别忘了给出升级文档，指导老用户如何迁移到新版本。还有一个更好的做法是，在我们开启一个新版本之际，就确定好上一个版本的寿命终点，提前知会到用户。

还有一个技术上的注意事项，那就是：**大版本间最好有明确的隔离**。对于一个复杂的技术产品来说，API 只是最终直接面向用户的接口，背后还有特定的环境、工具组、依赖包等各种支撑，互相之间并不能混用。

比如，曾经的经典前端库 KISSY。在业界技术方案日新月异的大潮下，KISSY 6 版本已经强依赖了 TNPM（阿里内网的 NPM）、DEF 套件组（淘宝 FED 的前端工具套件），虽然和之前的 1.4 版本相比 API 的变化并不大，但是仍然不能在老环境下直接使用 6 版本的代码库……这一定程度上降低了自由组合的灵活度，但事实上随着业务问题场景的复杂度提升，解决方案本身会需要更定制化，因此，将环境、工具等上下游关联物随代码一起打包，做成一整个技术方案，这正是业界的现状。

所以，隔离大版本，制定好 API 支持策略，让我们的产品更专业，让用户免去后顾之忧。

## 总结

以上，便是我从业以来感悟到的一些「道」，三个进阶层次、几十个细分要点，不知有没有给读者您带来一丁点启发。

但实际上，**大道至简**。我一直认为，程序开发和平时的说话写字其实没有太大区别，无非三者 ——

1. 逻辑和抽象。
2. 领域知识。
3. 语感。

写代码，就像写作，而设计 API 好比列提纲。勤写、勤思，了解前人的模式、套路，学习一些流行库的设计方法，掌握英语、提高语感……相信大家都能设计出卓越的 API。

最后，附上 API 设计的经典原则：

> Think about future, design with flexibility, but only implement for production.

## 引用

1. [Framework Design Guidelines](https://msdn.microsoft.com/en-us/library/ms229042(v=vs.110).aspx)
2. [Page Visibility 的 API 设计](https://github.com/lifesinger/blog/issues/164)
3. [我心目中的优秀 API](https://github.com/lifesinger/blog/issues/119)
4. [Clean Code JavaScript](https://github.com/ryanmcdermott/clean-code-javascript)

------

题图：[只是一张符合上下文的图片，并没有更深的含义](https://www.flickr.com/photos/132889348@N07/20013034943)。

花絮：由于文章很长，在编写过程中我也不由得发生了「同一个意思却使用多种表达方式」的情况。某些时候这是必要的 —— 可以丰富文字的多样性；而有些时候，则显得全文缺乏一致性。在发表本文之前，我搜索了这些词语：「调用者」、「调用方」、「引用者」、「使用者」，然后将它们统一修改为我们熟悉的名字：「用户」。

---



