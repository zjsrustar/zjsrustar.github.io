---
title: 使用 ES decorators 构建一致性 API「淘宝FED前端团队」
tags:
  - decorator
  - es7
categories: Web开发
typora-copy-images-to: building-consistent-api-with-es-decorators
typora-root-url: building-consistent-api-with-es-decorators
date: 2017-06-02 22:25:20
---


# 使用 ES decorators 构建一致性 API

作者: 法海 发表于: [2017-04-27](http://taobaofed.org/blog/2017/04/27/building-consistent-api-with-es-decorators/)

![1496413726350](1496413726350.png)

重用和一致性是程序设计中经久不衰的两个课题。在最新的 ES Proposal 中，「decorators 语法」为此带来了一定的便利，并且，很适合应用于大型的类库中。

------

## 装饰模式

提到 decorator 大家都不会陌生，即「装饰模式」—— 我们可以在「不侵入原有代码」的情况下，为代码增加一些「额外的功能」。

所谓「额外的功能」一般都比较独立，不和原有逻辑耦合，只是做一层包装。你也可以把它看成「包装模式」。形如：

```
// 旧方法
function func() {}

// 包装后的新方法
function funcWrapped() {

  // 有的没的
  doSomethingBefore();

  // 旧方法的过程本身并不变化
  func();

  // 这啊那的
  doSomethingAfter();
}

```

这样看来，有一些场景特别适用这个模式，比如：

- 记录方法的开始执行和结束执行。
- 为运算过程提供额外的缓存能力。
- 标记方法为 deprecated。
- 等等。

### 编写一个装饰器

如果有好多方法都想包上这种「额外的功能」，那么我们不会一个个地去改写，而是考虑抽出一个「装饰器」—— 它能够接受原方法，然后生成包装后的方法。比如，我们想记录所有方法的运行时间：

```
function performanceTimingDecorator(func) {

  // 返回包装后的新方法
  return function(...args) {
    const start = Date.now();
    func(...args);
    const end = Date.now();
    const t = end - start;

    console.log(`${func.name} performed ${t}ms.`);
  };
}

function func() {}

const funcWrapped = performanceTimingDecorator(func);

// func performed 2ms.
funcWrapped();

```

## 使用 ES decorators

如果一个系统内需要大量运用装饰器，那么上述的写法可读性还有待提高。ES decorators 解决了这个问题，这是一个新的语法（语法糖）：

```
// 定义 decorator
function performanceTiming(...args) {

  // 返回包装后的方法
  return function(target, key, descriptor) {
    // ...
  };
}

class MyClass {

  // 使用这种语法修饰方法 func
  @performanceTiming
  func() {}
}

```

新的 decorator 语法 `@xxx` 的形式非常类似 Java Annotation，不过后者作为静态语言，其 Annotation 的实现机制以及使用场景和 ES decorators 都有区别，这是一个题外话。事实上，ES decorators 完全借鉴自 Python 的 decorators。

同时，聪明的你应该发现，相比手写装饰器，新的语法中其实「该写的东西一个都没少」。那这个 decorators 语法有什么意义呢？

在我看来，这种语法糖对 decorators 的「定义」和「调用」都做了收敛，带来了「形式美感」。说人话，可读性更好。

1. 在 decorators 定义时，约束了装饰器的输入（固定的几个相关参数）和输出（返回一个 function），使所有装饰器风格得到收敛。
2. 在 decorators 调用时，以无侵入的语法「修饰」类或方法，可维护性和可读性都提升很多。

这两个优势，让我想到 ES decorators 的一个重要使用场景，便是应用于构架一致性 API。

## 构架一致性 API

对于多人开发的大型类库来说，「一致性」是很重要同时也很难执行的一个课题。这里的「一致性」包括：

1. 各模块提供一致的标准公用功能。
2. 公用功能的实现和调用方式也保持一致。
3. 整体 API 的风格一致。

其中 1、2 两点可以通过引入 ES decorators 机制来更好地达到。

### 实践演示

先封装好部分 decorators（可参见 `@ali/universal-decorator` 这个包），这里选取两个装饰器：

- `@deprecated` - 用于修饰类的方法，如果方法被调用，则在 console 中提示此方法已经过时，以便开发者转而调用其他方法。
- `@moduleLevel` - 这是 Rax 体系下模块类的一个静态成员标准字段，可取值为几个有限的枚举，此装饰器对此做了约束。

接下来具体地应用到库中。

例如 `@ali/universal-tracker` 中，`report()` 方法已经迁移到了 `@ali/universal-goldlog`，原方法已经废弃，则可以写作：

```
import {deprecated} from '@ali/universal-decorator';

class Tracker {

  @deprecated('This method is moved to universal-goldlog.', {
    url: 'http://web.npm.alibaba-inc.com/package/@ali/universal-goldlog'
  })
  report() {
    // ...
  }
}

```

然后在调用 `report()` 后则会提示：

![1496413764730](1496413764730.png)

这样，在相关的所有库中都引入类似的装饰器，从而保证 API 表达上的一致，并且这些公共逻辑遵循一致的实现。

另外还有一个例子，可以用来对类的字段做约束。以大量基于 Rax 的页面模块为例，这些模块 class 需要声明一个静态属性 `moduleLevel` 是 app 级别还是 page 级别，以便于框架将其渲染到对应的容器中。但是静态成员的赋值不够清晰明朗，也不能对枚举值做约束。使用 decorators 来改写则：

```
import {moduleLevel} from '@ali/universal-decorator';

@moduleLevel('page')
class MyModule1 {}

@moduleLevel('other')
class MyModule2 {}

```

moduleLevel 这个 decorator 将为类赋上一个名为 `moduleLevel` 的静态成员，并且会对传入值作判断，如果入参不是 `'page'` 或 `'app'`，则发出警告：

![1496413785396](1496413785396.png)

最后，由于使用了 ES decorators 语法的代码，类似于一种声明式的标记，所以更利于我们对这些代码作静态分析，比如进一步的提前校验，或是条件编译等等。这部分更多的想法和思路，有待发掘。

## 引用

1. [Exploring EcmaScript Decorators](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841)

------

题图：[一棵被装饰得五光十色的圣诞树](http://maxpixel.freegreatpicture.com/Festival-The-Christmas-Tree-Decorate-A-Christmas-Tree-1081320)。很多涉及到 decorator 的文章动不动就拿圣诞树来举例子，俨然 Christmas tree 是 decorate 的固定宾语。🎄

---
