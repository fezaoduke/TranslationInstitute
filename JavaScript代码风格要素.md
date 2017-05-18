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

