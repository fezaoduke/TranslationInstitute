[原文地址](https://medium.com/javascript-scene/elements-of-javascript-style-caa8821cb99f)

译者：墨白  校对：野草

------
![Out of the Blue — Iñaki Bolumburu (CC BY-NC-ND 2.0)](https://cdn-images-1.medium.com/max/800/1*7qYONdlJuS0pkUpdav-LQQ.jpeg)

1920年，由威廉·斯特伦克(William Strunk jr .)撰写的[英语写作手册:风格的要素(The Elements of Style)](https://www.amazon.cn/dp/B01M59NZUB/ref=cngwdyfloorv2_recs_0?pf_rd_p=7645736c-6759-4677-9dfb-2a3fd04770aa&pf_rd_s=desktop-2&pf_rd_t=36701&pf_rd_i=desktop&pf_rd_m=A1AJ19PSB66TGU&pf_rd_r=8ETX103G0HH940XG9RVK&pf_rd_r=8ETX103G0HH940XG9RVK&pf_rd_p=7645736c-6759-4677-9dfb-2a3fd04770aa)出版了，这本书为英文写作提供了经典的准绳。你可以通过在自己的编码风格中应用类似的标准来提高自己的编码水平。

下面只是一些准绳，但不是一成不变的法则。（There may be valid reasons to deviate from them if doing so clarifies the meaning of the code）如果能够很好的阐释代码的含义，那么我们也可以合理的避开这些规则，但是需要特别注意以及时刻自省。这些准绳经受住了时间的考验，有充分的理由证明：它们通常是正确的。如果要避开这些规则，一定要有c充足的理由，而不是简单地凭一时的兴趣或者个人的风格偏好。

几乎每一条文字语句组合的准则都适用于源代码编写：
- 以段落为基本单位：一段文字，一个主题。
- 删减无用的语句。
- 使用主动语态。
- 避免一连串松散的句子。
- 相关的词语写在一起。（keep related words together）
- 从正面去发表陈述。（put statements in positive form）
- 不同的概念采用不同的结构去阐述。（use parallel construction on parallel concepts）

我们可以应用相似的理念到代码编写风格上面：
1. 一个函数只做一件事，让函数成为代码组合的最小单元。
2. 删除不必要的代码。
3. 使用主动语态。
4. 避免一连串结构松散的，不知所云的代码。
5. 将功能相连的代码写在一起。（keep related code together）
6. 通过判断true值的方式来编写代码以及语句。（这句有点超前，是自己通读全文之后的理解 put statements and expressions in positive form）
7. 不同的需求通过不同的代码来实现。（use parallel construction on parallel concepts）

### 1.一个函数只做一件事，让函数成为代码组合的最小单元

> 软件开发的本质是“组合”。 我们通过组合模块，函数和数据结构来构建软件。理解如果编写以及组合函数是软件开发人员的基本技能。

模块是一个或多个函数或数据结构的简单集合，我们用数据结构来表示程序状态，只有在触发函数执行之后，程序状态才会发生有趣的变化。

JavaScript中，存在三种函数：
- 通信函数：函数用来执行I/O。
- 指令式函数：对一系列的声明列表进行分组。（a list of instructions,grouped together.）
- 映射式函数：给定一些输入，返回对应的输出。

在所有有效的应用程序都需要I/O，并且很多程序都遵循一定的程序执行顺序（procedural sequences）这种情况下，你的大部分函数应该是映射式函数：给定一些输入，该函数将返回一些相应的输出。

**每个函数只做一件事情**：如果你的函数主要用于I/O，请勿将I/O与映射（计算）混用。如果你的函数主要用于映射，请勿将其与I/O混用。根据定义，指令式函数违反了这一指导准则。指令式函数也违反了另一个指导准则：避免一连串结构松散，不知所云的代码。

理想中的函数是一个简单的、明确的纯函数：
- 同样的输入，总是返回相同的输出。
- 无副作用。

也可以查看，[“什么是纯函数？”](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)

### 2. 删除不必要的代码

简洁的代码对于软件而言至关重要。更多的代码意味更多的bug隐藏空间。*更少的代码 = 更少的bug隐藏空间 = 更少的bug*

简洁的代码更清晰，因为它拥有更高的“信噪比”：读者必须筛选出较少的“语法噪声”的代码来理解其含义。*更少的代码 = 更好的语法噪声 = 更强的代码含义信息传达*

借用《风格的元素》这本书里面的一句话就是：简洁的代码更具活力。

```
function secret (message) {
  return function () {
    return message;
  }
};
```

可以缩略成：
```
const secret = msg => () => msg;
```

对于那些熟悉简洁箭头函数写法的开发来说，可读性更好。它省略了不必要的语法：大括号，`function`关键字以及`return`语句。

第一种写法包含了不必要的语句。大括号，`function`关键字，以及`return`语句，这些对于熟悉箭头函数语法的开发来说，没有任何意义。它存在的唯一意义只是让那些不熟悉ES6语法的开发者更好的理解代码。

ES6自2015年已经成为语言标准，是时候去学习它了。

#### 删除不必要的代码
有时候，我们试图为无需称谓的事物命名。问题是人类的大脑在工作记忆中可用的资源有限，每个名称都必须作为一个单独的变量存储，占据一个可用的工作中的“记忆插槽点”。

由于这个原因，有经验的开发者懂得删除不必要的变量。（原文为learn to，但根据上下文的理解，应该是“懂得”）

例如，大多数情况下，你应该省略仅仅用来当做返回值的变量。你的函数名应该已经提供了关于函数返回值明确的信息。考虑以下：

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

另一个开发者正常用来减少变量名的做法是，利用函数组合以及`point-free`风格。

`Point-free`风格是一种定义函数方式，定义的函数无需引用参数。实现`point-free`风格常用的方式包括库里化以及函数组合。

让我们来看一个使用库里函数的例子：

```
const add2 = a => b => a + b;
// Now we can define a point-free inc()
// that adds 1 to any number.
const inc = add2(1);
inc(3); // 4
```

看一下`inc()`函数的定义方式。注意，它并未使用`function`关键字，或者`=>`语句。没有接受一系列的参数，因为该函数不在其内部处理一系列的参数。相反，它返回了一个知道如何处理参数的函数。

让我们来看另一个使用函数组合的例子。函数组合是将一个函数应用于另一函数运行结果的过程。 无论你是否意识到，你一直使用函数组合。 例如，当你链式调用如`.map()`或`promise.then()`时，你就是在使用它。大多数情况下，它看起来像这样：f(g(x))。

当你运用两个函数组合时，你无须创建一个变量来保存两个函数运行时的中间值。我们来看看函数组合是怎么减少代码的：

```
const g = n => n + 1;
const f = n => n * 2;
// With points:
const incThenDoublePoints = n => {
  const incremented = g(n);
  return f(incremented);
};
incThenDoublePoints(20); // 42
// compose2 - Take two functions and return their composition
const compose2 = (f, g) => x => f(g(x));
// Point-free:
const incThenDoublePointFree = compose2(f, g);
incThenDoublePointFree(20); // 42
```
你可以利用函子（functor）来做同样的事情。函子指的是你可以使用map来处理的事物。让我们利用函数组合的map链来写另一个版本的`compose2`：
```
const compose2 = (f, g) => x => [x].map(g).map(f).pop();
const incThenDoublePointFree = compose2(f, g);
incThenDoublePointFree(20); // 42
```

当你每次使用promise链式写法的时候，你就是在做同样的事情。

几乎每一个函数式编程类库都至少有两个版本的实用的组合函数：`compose()`，从右到左依次运行函数；`pipe()`，从左到右依次运行函数。

Lodash将这两个函数命名为`compose()`以及`flow()`。当我从Lodash引用它们的时候，我是这样引入的：
```
import pipe from 'lodash/fp/flow';
pipe(g, f)(20); // 42
```

下面的代码做着同样的事情，然而，代码量相比而言并未增加太多：
```
const pipe = (...fns) => x => fns.reduce((acc, fn) => fn(acc), x);
pipe(g, f)(20); // 42
```

如果函数组合这个名词听起来很陌生，你不知道如何使用它，请仔细想一想：
> 软件开发的本质是组合，我们通过组合较小的模块，方法以及数据结构来构建应用程序。

由此，可以总结出，理解函数和对象组合这一编程技巧就如同房地产建造商理解钻孔机以及气枪一样重要。

当你利用“命令式”代码（imperative code）将功能以及中间变量拼凑在一起时，就像疯狂使用胶带和胶水将这些部分粘贴起来一样。

记住：
- 如果能用更少的代码表达同样的意思，并且不会改变或者模糊化其所表达的含义，你应该采用这种方法。
-如果能用更少的变量表达相同的意思，并且不会改变或者模糊化其所表达的含义，你应该采用这种方法。

### 3. 使用主动语态

多直接命名事物。
- `myFunction.wasCalled()`优于`myFunction.hasBeenCalled()`
- `createUser`优于`User.create()`
- `notify()`优于`Notifier.doNotification()`

如果返回值为布尔值，命名中最好直接反应其取值:（Name predicts and booleans as if they are yes or no questions）
- `isActive(user)`优于`getActiveStatus(user)`
- `isFirstRun = false;`优于`firstRun = false;`

函数命名采用动词形式：
- `increment()`优于`plusOne()`
- `unzip()`优于`filesFromZip()`
- `filter(fn, array)`优于`matchingItemsFromArray(fn, array)`

#### 事件触发
事件触发以及生命周期函数由于被用作限定符，就不适用动词形式这一规则；相比于表达用来做什么，它们主要用来表示什么时候做。就如同它们被读的方式，“<什么时候去做>，<动作>”，这样命名。
- `element.onClick(handleClick)`由于`element.click(handleClick)`
- `element.onDragStart(handleDragStart)`优于`component.startDrag(handleDragStart)`

在第二种方式中，看起来我们正在尝试去触发一个事件，而不是对其作出回应。

#### 生命周期函数

对于组件生命周期方法（组件更新之前调用的方法），考虑一下以下的不同的命名：
- `componentWillBeUpdated(doSomething)`
- `componentWillUpdate(doSomething)`
- `beforeUpdate(doSomething)`

第一个示例中，我们使用了被动语态（将要被更新而不是将要更新）。这种方式很口语化，也没有更加比其它两种方式更清晰。

第二种方式好多了，但生命周期函数的重点在于触发处理事件。`componentWillUpdate(handler)`读起来就好像它将会更新一个处理时间，但这不是我们想要表达的。我们想说，“在组件更新之前，触发事件”。`beforeComponentUpdate()`能更清楚的表达这一想法。


更细致一点。因为这些方法所作用的对象（组件）都是内置的。在方法名中引用这些对象是多余的。想一想如果你直接调用这些方法时如何阅读：`component.componentWillUpdate()`。这就好像在说，“吉米吉米会在晚餐吃牛排。”你没有必要听到这些对象的名字两次。
- `component.beforeUpdate(doSomething)`由于`component.beforeComponentUpdate(doSomething)`

**函数混合**是指将方法作为属性或者函数添加到一个对象上面。方法将在同一个流中被依次调用-如同装配线。每一个函数混合将`instance`作为入参，并在其调用流中的下一个方法之前，将一些东西加在它上面。

我喜欢用形容词来命名函数混合。你也可以经常使用"ing"或者"able"后缀来找到有意义的形容词。例如：
- `const duck = composeMixins(flying, quacking);`
- `const box = composeMixins(iterable, mappable);`

### 4.避免一连串结构松散的，不知所云的代码
开发人员经常将一系列事件串联在一个进程中：一组松散的、相关度不高的语句被设计依次运行。过多的程序代码是形成“意大利面条”代码的原因。

这种程序序列经常通过相同的形式重复调用，每一个单独看起来都是巧妙的，有时意外地报错。例如，用户界面组件与几乎所有其他的用户界面组件共享相同的核心需求。 其关注点可以分解成生命周期阶段，并由单独的函数方法进行管理。

考虑一下的运行序列：
```
const drawUserProfile = ({ userId }) => {
  const userData = loadUserData(userId);
  const dataToDisplay = calculateDisplayData(userData);
  renderProfileData(dataToDisplay);
};
```

这个方法处理了三件不同的事：获取数据，根据获取的数据计算view的状态，以及渲染。

在大部分现代前端应用架构，这些关注点中的每一个都应该考虑分拆开。通过分拆这些关注点，我们可以轻松地为每个问题提供不同的函数。

例如，我们可以完全替换渲染器，它不会影响程序的其他部分，例如，React的丰富的自定义渲染器：适用于原生iOS和Android应用程序的ReactNative，WebVR的AFrame，用于服务器端渲染的ReactDOM/Server 等等...

此方法的另一个问题就是你不能在没有先加载数据的情况下，简单地计算要展示的数据并生成标签。如果你已经加载了数据怎么办？你可以结束没有必要继续做下去的工作。（What if you’ve already loaded the data? You end up doing work that you didn’t need to do in subsequent calls.）

分拆关注点也使得它们更容易进行测试。我喜欢对我的应用程序进行单元测试，并在写代码时展示每次代码改变之后的测试结果。但是，如果我们将渲染代码绑定到数据加载代码，我不能简单地将一些假数据传递给渲染代码进行测试。我必须从端到端测试整个组件-这个过程，由于浏览器加载，异步I/O请求等等会耗费时间。

我不能从我的单元测试中得到即时反馈。分拆功能点允许你进行单独的单元测试。

上述示例已经分拆出单独的功能点，我们可以在应用程序中提供不同的生命周期钩子给其调用。 当应用程序开始装载组件时，可以触发数据加载。可以根据响应视图状态更新来触发计算和渲染。

结果是软件的职责进一步明确：每个组件可以复用相同的结构和生命周期钩子，并且软件性能更好。在后续开发中，我们不需要重复的做相同的事。

### 5.功能相连的代码写在一起
许多框架以及boilerplates规定了程序组织的方法，其中文件按照技术类型分组。如果你正在构建一个小的计算器获取一个待办事宜的app，这杨作是很好的。但是对于较大的项目，通过功能特性将文件分组在一起是更好的方法。

按类型分组：
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

按特性分组：
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

当你通过功能特性来将文件分组，你可以避免在为了补全功能在文件列表上下滚动，之后查找编辑所需要的文件这种情况。

> 共享相关的功能特性文件（colocate files related by feature）

### 6.通过判断true值的方式来编写代码以及语句
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

#### 尽量采用强烈的负面含义的命名
有时候我们只关心一个变量是否缺失，因此如果使用是一个true值的名字，那么我们将不得不利用`!`操作符来否定它。这些情况下，尽量采用强烈的负面含义的命名。`not`以及`!`操作符会创建弱的表达式。
- `if (missingValue)`优于`if (!hasValue)`
- `if (anonymous)`优于`if (!user)`
- `if (!isEmpty(thing))`优于`if (notDefined(thing))`
