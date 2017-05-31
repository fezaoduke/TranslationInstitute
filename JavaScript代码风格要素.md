[原文地址](https://medium.com/javascript-scene/elements-of-javascript-style-caa8821cb99f)

译者：墨白  校对：野草

------
![Out of the Blue — Iñaki Bolumburu (CC BY-NC-ND 2.0)](https://cdn-images-1.medium.com/max/800/1*7qYONdlJuS0pkUpdav-LQQ.jpeg)

1920年，由威廉·斯特伦克(William Strunk jr .)撰写的[《英语写作手册:风格的要素(The Elements of Style)》](https://www.amazon.cn/dp/B01M59NZUB/ref=cngwdyfloorv2_recs_0?pf_rd_p=7645736c-6759-4677-9dfb-2a3fd04770aa&pf_rd_s=desktop-2&pf_rd_t=36701&pf_rd_i=desktop&pf_rd_m=A1AJ19PSB66TGU&pf_rd_r=8ETX103G0HH940XG9RVK&pf_rd_r=8ETX103G0HH940XG9RVK&pf_rd_p=7645736c-6759-4677-9dfb-2a3fd04770aa)出版了，这本书列举了7条英文写作的准则，过了一个世纪，这些准则并没有过时。对于工程师来说，你可以在自己的编码风格中应用类似的建议来指导日常的编码，提高自己的编码水平。

需要注意的是，这些准则不是一成不变的法则。如果违背它们，能够让代码可读性更高，那么便没有问题，但请特别小心并时刻反思。这些准绳是经受住了时间考验的，有充分的理由说明：它们通常是正确的。如果要违背这些规则，一定要有充足的理由，而不要单凭一时的兴趣或者个人的风格偏好。

书中的写作准则如下：
- 以段落为基本单位：一段文字，一个主题。
- 删减无用的语句。
- 使用主动语态。
- 避免一连串松散的句子。
- 相关的内容写在一起。
- 从正面利用肯定语句去发表陈述。
- 不同的概念采用不同的结构去阐述。

我们可以应用相似的理念到代码编写上面：
1. 一个function只做一件事，让function成为代码组合的最小单元。
2. 删除不必要的代码。
3. 使用主动语态。
4. 避免一连串结构松散的，不知所云的代码。
5. 将相关的代码写在一起。
6. 利用判断true值的方式来编写代码。
7. 不同的技术方案利用不同的代码组织结构来实现。

### 1.一个function只做一件事，让function成为代码组合的最小单元

> 软件开发的本质是“组合”。 我们通过组合模块，函数和数据结构来构建软件。理解如果编写以及组合方法是软件开发人员的基本技能。

模块是一个或多个function和数据结构的简单集合，我们用数据结构来表示程序状态，只有在函数执行之后，程序状态才会发生一些有趣的变化。

JavaScript中，可以将函数分为3种：
- I/O 型函数 (Communicating Functions)：函数用来执行I/O。
- 过程型函数 (Procedural Functions)：对一系列的指令序列进行分组。
- 映射型函数 (Mapping Functions)：给定一些输入，返回对应的输出。

有效的应用程序都需要I/O，并且很多程序都遵循一定的程序执行顺序，这种情况下，程序中的大部分函数都会是映射型函数：给定一些输入，返回相应的输出。

**每个函数只做一件事情**：如果你的函数主要用于I/O，就不要在其中混入映射型代码，反之亦然。严格根据定义来说，过程型函数违反了这一指导准则，同时也违反了另一个指导准则：避免一连串结构松散，不知所云的代码。

理想中的函数是一个简单的、明确的纯函数：
- 同样的输入，总是返回同样的输出。
- 无副作用。

也可以查看，[“什么是纯函数？”](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)

### 2. 删除不必要的代码

简洁的代码对于软件而言至关重要。更多的代码意味更多的bug隐藏空间。*更少的代码 = 更少的bug隐藏空间 = 更少的bug*

简洁的代码读起来更清晰，因为它拥有更高的“信噪比”：阅读代码时更容易从较少的语法噪音中筛选出真正有意义的部分。可以说，*更少的代码 = 更少的语法噪声 = 更强的代码含义信息传达*

借用《风格的元素》这本书里面的一句话就是：简洁的代码更健壮。

```
function secret (message) {
  return function () {
    return message;
  }
};
```

可以简化成：
```
const secret = msg => () => msg;
```

对于那些熟悉简洁箭头函数写法的开发来说，可读性更好。它省略了不必要的语法：大括号，`function`关键字以及`return`语句。

而简化前的代码包含的语法要素对于传达代码意义本身作用并不大。它存在的唯一意义只是让那些不熟悉ES6语法的开发者更好的理解代码。

ES6自2015年已经成为语言标准，是时候去学习它了。

#### 删除不必要的代码
有时候，我们试图为不必要的事物命名。问题是人类的大脑在工作中可用的记忆资源有限，每个名称都必须作为一个单独的变量存储，占据工作记忆的存储空间。

由于这个原因，有经验的开发者会尽可能地删除不必要的变量。

例如，大多数情况下，你应该省略仅仅用来当做返回值的变量。你的函数名应该已经说明了关于函数返回值的信息。看看下面的：

```
const getFullName = ({firstName, lastName}) => {
  const fullName = firstName + ' ' + lastName;
  return fullName;
};
```

对比

```
const getFullName = ({firstName, lastName}) => (
  firstName + ' ' + lastName
);
```

另一个开发者通常用来减少变量名的做法是，利用函数组合以及`point-free-style`。

`Point-free-style`是一种定义函数方式，定义成一种与参数无关的合成运算。实现`point-free`风格常用的方式包括函数科里化以及函数组合。

让我们来看一个函数科里化的例子：

```
const add2 = a => b => a + b;
// Now we can define a point-free inc()
// that adds 1 to any number.
const inc = add2(1);
inc(3); // 4
```

看一下`inc()`函数的定义方式。注意，它并未使用`function`关键字，或者`=>`语句。add2也没有列出一系列的参数，因为该函数不在其内部处理一系列的参数，相反，它返回了一个知道如何处理参数的新函数。

函数组合是将一个函数的输出作为另一函数的输入的过程。 也许你没有意识到，你一直在使用函数组合。链式调用的代码基本都是这个模式，比如数组操作时使用的`.map()`，Promise 操作时的`promise.then()`。函数组合在函数式语言中也被称之为高阶函数，其基本形式为：f(g(x))。

当两个函数组合时，无须创建一个变量来保存两个函数运行时的中间值。我们来看看函数组合是怎么减少代码的：

```
const g = n => n + 1;
const f = n => n * 2;
// 需要操作参数、并且存储中间结果
const incThenDoublePoints = n => {
  const incremented = g(n);
  return f(incremented);
};
incThenDoublePoints(20); // 42

// compose2 - 接受两个函数作为参数，直接返回组合
const compose2 = (f, g) => x => f(g(x));
const incThenDoublePointFree = compose2(f, g);
incThenDoublePointFree(20); // 42
```
你可以利用函子（functor）来做同样的事情。在函子中把参数封装成可遍历的数组。让我们利用函子来写另一个版本的`compose2`：
```
const compose2 = (f, g) => x => [x].map(g).map(f).pop();
const incThenDoublePointFree = compose2(f, g);
incThenDoublePointFree(20); // 42
```

当每次使用promise链时，你就是在做这样的事情。

几乎每一个函数式编程类库都提供至少两种函数组合方法：从右到左依次运行的`compose()`；从左到右依次运行的`pipe()`。

Lodash中的`compose()`以及`flow()`分别对应这两个方法。下面是使用`pipe`的例子：
```
import pipe from 'lodash/fp/flow';
pipe(g, f)(20); // 42
```

下面的代码也做着同样的事情，但代码量并未增加太多：
```
const pipe = (...fns) => x => fns.reduce((acc, fn) => fn(acc), x);
pipe(g, f)(20); // 42
```

如果函数组合这个名词听起来很陌生，你不知道如何使用它，请仔细想一想：
> 软件开发的本质是组合，我们通过组合较小的模块，方法以及数据结构来构建应用程序。

不难推论，工程师理解函数和对象组合这一编程技巧就如同搞装修需要理解钻孔机以及气枪一样重要。

当你利用“命令式”代码将功能以及中间变量拼凑在一起时，就像疯狂使用胶带和胶水将这些部分胡乱粘贴起来一样，而函数组合看上去更流畅。

记住：
- 用更少的代码。
- 用更少的变量。

### 3. 使用主动语态

主动语态比被动语态更直接，跟有力量，尽量多直接命名事物：
- `myFunction.wasCalled()`优于`myFunction.hasBeenCalled()`
- `createUser`优于`User.create()`
- `notify()`优于`Notifier.doNotification()`

命名布尔返回值时最好直接反应其输出的类型:
- `isActive(user)`优于`getActiveStatus(user)`
- `isFirstRun = false;`优于`firstRun = false;`

函数名采用动词形式：
- `increment()`优于`plusOne()`
- `unzip()`优于`filesFromZip()`
- `filter(fn, array)`优于`matchingItemsFromArray(fn, array)`

#### 事件处理
事件处理以及生命周期函数由于是限定符，比较特殊，就不适用动词形式这一规则；相比于“做什么”，它们主要用来表达“什么时候做”。对于它们，可以“<什么时候去做>，<动作>”这样命名，朗朗上口。
- `element.onClick(handleClick)`优于`element.click(handleClick)`
- `element.onDragStart(handleDragStart)`优于`component.startDrag(handleDragStart)`

上面两例的后半部分，它们读起来更像是正在尝试去触发一个事件，而不是对其作出回应。

#### 生命周期函数

对于组件生命周期函数（组件更新之前调用的方法），考虑一下以下的命名：
- `componentWillBeUpdated(doSomething)`
- `componentWillUpdate(doSomething)`
- `beforeUpdate(doSomething)`

第一个种我们使用了被动语态（将要被更新而不是将要更新）。这种方式很口语化，但含义表达并没有比其它两种方式更清晰。

第二种就好多了，但生命周期函数的重点在于触发处理事件。`componentWillUpdate(handler)`读起来就好像它将立即触发一个处理事件，但这不是我们想要表达的。我们想说，“在组件更新之前，触发事件”。`beforeComponentUpdate()`能更清楚的表达这一想法。

进一步简化，因为这些方法都是组件内置的。在方法名中加入component是多余的。想一想如果你直接调用这些方法时：`component.componentWillUpdate()`。这就好像在说，“吉米吉米在晚餐吃牛排。”你没有必要听到同一个对象的名字两次。显然，
- `component.beforeUpdate(doSomething)`优于`component.beforeComponentUpdate(doSomething)`

**函数混合**是指将方法作为属性添加到一个对象上面，它们就像装配流水线给传进来的对象加上某些方法或者属性。

我喜欢用形容词来命名函数混合。你也可以经常使用"ing"或者"able"后缀来找到有意义的形容词。例如：
- `const duck = composeMixins(flying, quacking);`
- `const box = composeMixins(iterable, mappable);`

### 4.避免一连串结构松散的，不知所云的代码
开发人员经常将一系列事件串联在一个进程中：一组松散的、相关度不高的代码被设计依次运行。从而很容易形成“意大利面条”代码。

这种写法经常被重复调用，即使不是严格意义上的重复，也只有细微的差别。例如，界面不同组件之间几乎共享相同的核心需求。 其关注点可以分解成不同生命周期阶段，并由单独的函数方法进行管理。

考虑以下的代码：
```
const drawUserProfile = ({ userId }) => {
  const userData = loadUserData(userId);
  const dataToDisplay = calculateDisplayData(userData);
  renderProfileData(dataToDisplay);
};
```

这个方法做了三件事：获取数据，根据获取的数据计算view的状态，以及渲染。

在大部分现代前端应用中，这些关注点中的每一个都应该考虑分拆开。通过分拆这些关注点，我们可以轻松地为每个问题提供不同的函数。

比如，我们可以完全替换渲染器，它不会影响程序的其他部分。例如，React的丰富的自定义渲染器：适用于原生iOS和Android应用程序的ReactNative，WebVR的AFrame，用于服务器端渲染的ReactDOM/Server 等等...

`drawUserProfile`的另一个问题就是你不能在没有数据的情况下，简单地计算要展示的数据并生成标签。如果数据已经在其他地方加载过了会怎么样，就会做很多重复和浪费的事情。

分拆关注点也使得它们更容易进行测试。我喜欢对我的应用程序进行单元测试，并在每次修改代码时查看测试结果。但是，如果我们将渲染代码和数据加载代码写在一起，我不能简单地将一些假数据传递给渲染代码进行测试。我必须从端到端测试整个组件。而这个过程中，由于浏览器加载，异步I/O请求等等会耗费时间。

上面的`drawUserProfile`代码不能从单元测试测试中得到即时反馈。而分拆功能点允许你进行单独的单元测试，得到测试结果。

上文已经已经分析出单独的功能点，我们可以在应用程序中提供不同的生命周期钩子给其调用。 当应用程序开始装载组件时，可以触发数据加载。可以根据响应视图状态更新来触发计算和渲染。

这么做的结果是软件的职责进一步明确：每个组件可以复用相同的结构和生命周期钩子，并且软件性能更好。在后续开发中，我们不需要重复相同的事。

### 5.功能相连的代码写在一起
许多框架以及boilerplates规定了程序文件组织的方法，其中文件按照代码类别分组。如果你正在构建一个小的计算器，获取一个待办事宜的app，这样做是很好的。但是对于较大的项目，通过业务功能特性将文件分组在一起是更好的方法。

按代码类别分组：
```
.
├── components
│   ├── todos
│   └── user
├── reducers
│   ├── todos
│   └── user
└── tests
    ├── todos
    └── user
```

按业务功能特性分组：
```
.
├── todos
│   ├── component
│   ├── reducer
│   └── test
└── user
    ├── component
    ├── reducer
    └── test
```

当你通过功能特性来将文件分组，你可以避免在文件列表上下滚动，查找编辑所需要的文件这种情况。


### 6.利用判断true值的方式来编写代码

要做出确定的断言，避免使用温顺、无色、犹豫的语句，必要时使用 not 来否定、拒绝。典型的
- `isFlying`优于`isNotFlying`
- `late`优于`notOneTime`

#### if语句
```
if (err) return reject(err);

// do something
```
优于
```
if (!err) {
  // ... do something
} else {
  return reject(err);
}
```

#### 三元判断语句
```
{
  [Symbol.iterator]: iterator ? iterator : defaultIterator
}
```
优于
```
{
  [Symbol.iterator]: (!iterator) ? defaultIterator : iterator 
}
```

#### 恰当的使用否定
有时候我们只关心一个变量是否缺失，如果通过判断true值的方式来命名，我们得用`!`操作符来否定它。这种情况下使用 "not" 前缀和取反操作符不如使用否定语句直接。
- `if (missingValue)`优于`if (!hasValue)`
- `if (anonymous)`优于`if (!user)`
- `if (!isEmpty(thing))`优于`if (notDefined(thing))`

#### 函数调用时，避免用null以及undefined代替某一个参数

不要在函数调用时，传入`undefined`或者`null`作为某个参数的值。如果某些参数可以缺失，更推荐传入一个对象：
```
const createEvent = ({
  title = 'Untitled',
  timeStamp = Date.now(),
  description = ''
}) => ({ title, description, timeStamp });

const birthdayParty = createEvent({
  title: 'Birthday Party',
  description: 'Best party ever!'
});
```
优于
```
const createEvent = (
  title = 'Untitled',
  timeStamp = Date.now(),
  description = ''
) => ({ title, description, timeStamp });

const birthdayParty = createEvent(
  'Birthday Party',
  undefined, // This was avoidable
  'Best party ever!'  
);
```
#### 不同的技术方案利用不同的代码组织结构来实现

迄今为止，应用程序中未解决的问题很少。最终，我们都会一次又一次地做着同样的事情。当这样的场景发生时，意味着代码重构的机会来啦。分辨出类似的部分，然后抽取出能够支持每个不同部分的公共方法。这正是类库以及框架为我们做的事情。

UI组件就是一个很好的例子。10 年前，使用 jQuery 写出把界面更新、应用逻辑和数据加载混在一起的代码是再常见不过的。渐渐地，人们开始意识到我们可以将MVC应用到客户端的网页上面，随后，人们开始将model与UI更新逻辑分拆。

最终，web应用广泛采用组件化这一方案，这使得我们可以使用JSX或HTML模板来声明式的对组件进行建模。

最终，我们就能用完全相同的方式去表达所有组件的更新逻辑、生命周期，而不用再写一堆命令式的代码

对于熟悉组件的人，很容易看懂每个组件的原理：利用标签来表示UI元素，事件处理器用来触发行为，以及用于添加回调的生命周期钩子函数，这些钩子函数将在必要时运行。

当我们对于类似的问题采用类似的模式解决时，熟悉这个解决模式的人很快就能理解代码是用来做什么的。

### 结论：代码应该简单而不是过于简单化

尽管在2015，ES6已经标准化，但在2017，很多开发者仍然拒绝使用ES6特性，例如箭头函数，隐式return，rest以及spread操作符等等。利用自己熟悉的方式编写代码其实是一个幌子，这个说法是错误的。只有不断尝试，才能够渐渐熟悉，熟悉之后，你会发现简洁的ES6特性明显优于ES5：与语法结构偏重的ES5相比，简洁的es6的代码很简单。

代码应该简单，而不是过于简单化。

简洁的代码有以下优势：
- 更少的bug可能性
- 更容易去debug

但也有如下弊端：
- 修复bug的成本更高
- 有可能引用更多的bug
- 打断了正常开发的流程

简洁的代码同样：
- 更易写
- 更易读
- 更好去维护

清楚自己的目标，不要毫无头绪。毫无头绪只会浪费时间以及精力。投入精力去训练，让自己熟悉，去学习更好的编程方式，以及更有更有活力的代码风格。

> 代码应该简单，而不是简单化。
