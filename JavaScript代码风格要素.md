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
- 程序性函数：对一系列的声明列表进行分组。（a list of instructions,grouped together.）
- 映射性函数：给定一些输入，返回对应的输出。

在所有有效的应用程序都需要I/O，并且很多程序都遵循一定的程序执行顺序（procedural sequences）这种情况下，你的大部分函数应该是映射性函数：给定一些输入，该函数将返回一些相应的输出。

**每个函数只做一件事情**：如果你的函数主要用于I/O，请勿将I/O与映射（计算）混用。如果你的函数主要用于映射，请勿将其与I/O混用。根据定义，程序性函数违反了这一指导准则。程序性函数也违反了另一个指导准则：避免一连串结构松散，不知所云的代码。

理想中的函数是一个简单的、明确的纯函数：
- 同样的输入，总是返回相同的输出。
- 无副作用。

也可以查看，[“什么是纯函数？”](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)

### 2. 删除不必要的代码

