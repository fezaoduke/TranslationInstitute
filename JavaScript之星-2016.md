JavasScript社区在创新的道路上开足了马力，曾经流行过的也许一个月之后就过时了。

2016已经结束了。你可能会想你是否错过一些重要的东西？不用担心，我们正在回顾那些流行趋势。

通过比较过去12个月里Github所增加的star数，我们依次来看看哪些项目吸引了大多数眼光。

在2015，React是当之无愧的王者，Redux赢得了与flux之间的战争，攻陷了Flux的城池。

那么，谁会是2016年的JavaScript之星呢？

我们利用[bestof.js.org](bestof.js.org)分析所涉及的项目技术。（bestof.js.org罗列了一系列与web平台相关的最好的项目）

### 1. 2016最流行项目
通过比较去年最火的10个项目，你可以总览2016的web前端技术发展，会发现：
- 3个UI框架：[Vue.js](http://vuejs.org/), [React](https://facebook.github.io/react/)以及[Angular 2](https://angular.io/)
- 新的node.js包管理工具：[Yarn](https://yarnpkg.com/)
- 桌面应用领先级解决方案：[Electron](http://electron.atom.io/)
- 快速开始React解决方案：[Create React App](https://github.com/facebookincubator/create-react-app)
- 移动端框架：[React Native](http://facebook.github.io/react-native/)
- 最著名的CSS工具包：[Bootstrap](http://getbootstrap.com/)
- 基于函数式编程思想的状态管理库：[Redux](http://redux.js.org/)
- 强大灵活的图表库：[D3](https://d3js.org/)

这彰显了2016年JavaScript应用的全面性以及多功能性。

##### 2016年的王者是······
Vue.JS在去年获得了超过25000个star，这意味着每天有72个star，超过了包含React以及Angular在内的其它任何框架。

Vue.JS在10月发布了2.0版本，这一版本为了更好的表现加入了Virtual DOM渲染方式。

Vue.JS已经在很多大公司的生产环境投入使用（包括阿里巴巴，中国最大的电子商务公司），所以，你可以放心的使用它。

现在Vue.JS已经有一个相对成熟的生态体系，包括路由（vue-router）以及状态管理库（Vuex）。

看起来Vue.JS结合了React（组件化思路）以及Angular（html模板增强语法）的精华

### 2. 前端框架

这个前端框架清单可能是2016年JavaScript疲劳的元凶之一。似乎每个月都有一个新的竞争者出现并发出自己的声音，推动着JavaScript的创新。

准确的说，这个目录里面有两种不同类型的框架：
- 全能型框架，创建一个现代web应用所需的特性全部有，包括路由、数据获取、状态管理。AngularJS，Angular 2，Ember或者Aurelia都在这个类别里面。
- 专注于UI层面的轻便解决方案，例如React，Vue.JS，以及Inferno等等。

我们已经提到了Vue.JS(排名第1的那个)，让我们来看看其它的竞争者。

##### React以及它的竞争者
React排名第2，没有一个前端会忽视React以及它那丰富的生态圈。

React如此流行以至于其它的库总想取其精华，去其糟粕，在构建以及浏览器渲染方面提高效率。

Inferno是这个清单里面最流行的项目了，它声称最快的React替代品。

在我们的排行榜紧邻Inferno，Preact同样是React一个很好的替代品。它的生态系统相当成熟，举个例子，拥有离线缓存、路由以及兼容模板功能的样板，你可以利用这个样板使用你Preact项目里面的已存在的React库。

##### Angular 1 和 2
Angular如今已经分成2个仓库了，因为Angular2是对Angular1的全部重写，尽管有一些概念仍然相同。

Angular 2 基于 TypeScript 以及 ES6 编写，使其更加“现代”、“缜密”。

AngularJS这个项目指代的是其分支 1.x ，它仍然在许多项目中使用，并且仍会流行一段时间。

现在，是时候提及Ember了，虽然它庞大的生态圈以及社区不在top 10里面。

So it seems that, rather than opting for full frameworks with all features "out of the box", developers in 2016 flavored lighter approaches and prefer to compose their own solution "à la carte".
因此，看起来，相比于封装好的包含所有特性的全能型框架，2016年的开发者们更青睐轻量型框架，并且更愿意自定义解决方案，自己“点菜”。

### 3. Node.js 框架
在2016，从未如此简单去创建以及部署一个node.js应用，只需要通过：
- Now
- Webtask.io
- Stdlib

类似于 Gomix 的工程项目更是降低了进入 node.js 世界的门槛，它使任何人只需要通过一些点击操作就能够直接在浏览器编写以及分享 node.js 代码。

那么，如果你不得不写一个web应用，你会选择哪一个框架？

##### Express
当你想用 node.js 写一个web应用的时候，Express 经常会被认为web服务器的选择之一。大多数 node.js 开发者对于它的设计哲学（核心很小，但可以通过各种中间件进行拓展）非常熟悉。

##### Koa
Koa的设计哲学与 Express 类似，只不过它使用了 ES6 中的 generators 来避免回调地狱问题。

##### Feathers
对于以“服务端导向”架构设计，Feathers是一个非常灵活的解决方案，很适合用来构建小的 node.js 服务。

##### Nodal
Nodal框架立志于解决连接到PostgreSQL数据库的无状态以及分布服务问题。

##### Keystone
Keystone是最好的MongoDB数据管理以及运行终端解决方案之一，它主要是用来处理从MongoDB数据库传来的内容。
管理界面是直接从模型里面生成的，拥有所有的CRUD操作以及很好的过滤功能。

##### Sails
Sails是一个完全的MVC框架，灵感来源于Ruby on Rails（所以把它命名为**S**ails！）。它已经发布了挺长一段时间了，兼容各种版本的数据库，SQL或者no-SQL。

##### Loopback
Loopback是另一个拥有很多特性的成熟框架，其中包括需要token验证的权限以及兼容任何数据库。

最牛逼的特性是其API探测功能。这个特性让开发者以直观的方式检测所有的API端点，在此过程中，开发者还可以去检测所有用户的token。
如果你想要去开发一个新的API，它回事很好的选择。

### 4. React模板生成器
React是一个非常棒的UI库，但是利用React配合现代web开发工作流工具需要太多的配置。所以，怎么才能开始构建一个真实存在的应用呢？

React模板生成器以及其它的starter开发工具包给了我们这个答案。

##### 创建React应用
Facebook为了满足这个需求，提供了一种更轻便的路线，被称为 Create React App，利用它可以很方便的开始一个新的React项目。

Dan Abramov（Redux的创建者，目前在Facebook工作）为寻找构建简单性和自主配置性之间的平衡做了很大的贡献。
例如，不需要繁杂的样式解决方案（仅仅只是单纯的CSS），不需要服务端渲染，依旧可以将应用很好的打包，同时，开发体验也非常棒。

与其竞争者最主要的区别在于，如果你使用了 Create React App，它就会成为你项目中的一个依赖，你只会看到你的应用的代码，其它的黑魔法都被隐藏起来了。
当然，你可以自由选择合适升级依赖包，这并不仅仅是开始。

##### React模板生成器
被成为 React boilerplate 的生成器拥有你所需要的一切，包括 Redux 以及一些比较好的本地特性，很好地利用了web工作者的技术优势。

它允许开发者创建一个被称为渐进式web应用（或者PWA）：web应用本地运行，使用了一种被称为Service Worker的技术，你可以阅读Nicolás Bevacqua所写的[这篇文章](https://ponyfoo.com/articles/progressive-app-serviceworker)。

##### Next.js
Next.js，由来自Zeit忙碌的开发者所创建，拥有服务端渲染特性，可以通过此特性创建“万能应用”（或者我们在2015年所说的同构应用），
这代表应用不管是在客户端还是服务端都基本上可以运行同一套代码。

### 5. Mobile
JavaScript无处不在，现在你都可以使用web开发者的技术（HTML，JavaScript，CSS）来创建移动端应用。

##### React Native
利用React Native，基于相同的代码以及React开发者熟悉的理念方法，你可以构建iOS和Android不同平台的移动端应用。
想要了解更多关于构建iOS以及Andriod平台应用的知识，请移步官方教程。

其它的解决方案，基于Cordova，常常依赖于Webview去渲染页面，但并没有原生流畅。“Write Once Run Everywhere···”开发者们的梦想终于成真啦！

##### Ionic
Ionic是“hybrid”应用概念的先锋。在其外表之下，它基于Cordova来获取移动设备的各种特性。它非常成熟，拥有一个庞大的生态系统。

##### NativeScript
NativeScript与React Native目的（利用web技术开发真正的移动端应用）相同。它有两种形式，NativeScript Core以及NativeScript + Angular 2

以及······

2017年有一项快完成的项目：Weex，一个基于Vue.JS的跨平台移动端UI框架。

### 6. 编译器
我们现在这里讨论的编译器（或者“转换器”）是指其将其他语言（或者JavaScript的超集）转换JavaScript。它们将代码转换成“标准JavaScript”代码，即浏览器（或者node.js）可以执行的代码。

举个例子，编译器使开发者可以使用最新版本的JavaScript（ES6）而不需要担心浏览器兼容问题。

##### TypeScript
引领潮流的转换器是TypeScript，它为web开发者带来了Java以及C#开发者所使用的静态类型。Angular 2 正是基于TypeScript增加了许多的特性（traction）。
这里也存在在JavaScript中使用类型的优势与劣势，你可以读下面的两篇文章来思考自己的观点。
- [You Might Not Need TypeScript](https://medium.com/javascript-scene/you-might-not-need-typescript-or-static-types-aa7cb670a77b#.1pn05vlis)
- [TypeScript:the missing introduction](https://toddmotto.com/typescript-the-missing-introduction)

##### Babel
Babel，同Webpack，已经基本上成为一个的标准工具，用来编译ES6代码以及使用类似React（JSX）类库的模板。
最初，它只是用来编译ES6，现在已经成为一个用来做代码转换的更加通用的工具，感谢那一大批插件。

##### Flow
Flow并不是一个编译器，它只是一个静态类型检测工具，用来“标注”JavaScript代码。基本上，在代码中使用Flow只是增加对于意料中类型的注释
（[这里](http://javascriptplayground.com/blog/2017/01/npm-flowjs-javascript/)阅读更多关于使用Flow去写模块）

它被使用在Faceboook项目的代码中。自从Facebook成为开源世界（开源项目例如React，React Native，Flux， Immutable，Jest···）的主角之一，
这里面有太多的意义了。

##### CoffeeScript
在过去几年中，基于Python以及Ruby语法的CoffeeScript以及它精妙的语法成为最流行的编译器，但在2016，它不再那么流行，很多开发者由CoffeeScript转向搭配Babel的ES6。

### 7. 构建工具
在2016，很难想象一个web应用没有经过任何构建过程。你经常需要通过构建来转换模板以及优化资源，保证你的web应用在生产环境正常运行。

##### Wepack
Webpack是用来构建单页应用的主要工具，它与React生态良好兼容。最新发布的版本2增加一些先前期望的功能（可通过[这篇介绍](https://blog.madewithenvy.com/getting-started-with-webpack-2-ed2b86c68783#.7wyiawc0o)查看）

##### Gulp
Gulp是一个通用任务运行器，可以用来处理任何形式的自动化过程包括文件系统，所以，它并不是Webpack或者Browserify的直接竞争对手。

类似于Grunt，Gulp通过整合任务的方式工作：你可以让它压缩合并一系列的资源但它跟Webpack以及Browserify不同，并不处理模块化的JavaSript代码。

但它可以与Webpack很好的兼容，即使开发更愿意去使用npm scripts代替它。

### Browserify
Browserify，由于其简便性，很多node.js开发者喜欢它。

大体上，它会利用几个node.js包来生成一个简单的构建后的文件作为输出。但看起来，一个类似Webpack这种自主配置更强的工具更适用于web应用开发工作。

以及······

2017年一个模块化处理工具正在崛起，它更强调性能问题，那就是rollup。

它使用了具有被称作“Tree shaking”的特性ES6模块来创建bundle，里面只有在你代码中使用的函数，而不是一整个类库。

### 8. 测试框架
两个著名的测试框架使Jasmine以及 Mocha，但在2016，2个发布不久的项目吸引了很大的关注：AVA和Jest。

##### AVA
AVA，由多产的Sindre Sorhus创建，主要关注于性能（平行测试）以及ES6。AVA的语法与标准测试框架，例如Tape和Node-tap，相似。

##### Jest
Jest，又一个Facebook项目，在上个星期获得了很多的关注。在React社区，它很出名，越来越多的人开始向Jest迁移（看[这个故事](https://medium.com/@kentcdodds/migrating-to-jest-881f75366e7e#.z9x53j1ea)），
在2017年，它有可能成为最流行的测试框架。

Jest拥有很好的内置Mock数据能力，而其它的测试框架需要依赖类似于Sinon.js这样的类库。

### 9. IDE
关于IDE（整合开发环境），不得不提2个最受欢迎的利用web技术开发的开源IDE。

##### Visual Stdio Code
在我们的统计结果中，微软引领了Visual Stdio Code的开发工作。

它很好的整合了TypeScript以及node.js。一些开发者提到了开发速度的提升，这得感谢“IntelliSence”特性（整合了高亮以及自动补全）

提到“开源”和“微软”，再也不是矛盾的啦！

##### Atom
Atom，由Github主推，利用Electron创建（类似于其它的桌面应用，包括桌面客户端），其并不输于Visual Stdio Code太多。
关于Atom，有一个有趣的事情：它主要的编写语言是CoffeeScript！

### 10. 静态网页生成器
静态网页生成器（或者被称为“SSG”）用来生成一系列html，css以及JavaScript文件的工具。你可以将这些文件部署在一个简单的web服务器上面（Apache或者NGNX），
不需要感到惊奇或者设置一个数据库或者使用任何的web框架。正如Gatsby网站这么说：

> build sites like it's 1995

静态网页很快，很强大并且很容易去维护。

SSG非常流行，因为有很多种好的方法能够免费去建立网站：
- Github pages
- Gitlab pages
- Netlify
- Surge
- Now static

##### Hexo
在2016，利用node.js构建的最流行的SSG就是Hexo。它是一个一站式SSG，与Wordpress这样的CMS系统类似，
例如可以用来创建博客。它有很多特性包括国际化的插件。

##### Gatsby
新的入局者Gatsby是一个非常有趣的解决方案，它与它的竞争者不同，因为它利用React生态系统来生成静态html文件。
你可以选择与React组件、markdown文件以及服务端渲染整合，这使得它非常强大。

### 结论
尽管JavaScript疲劳以及各种戏剧性的事件（还记得“left-pad”门吧？），2016年社区发展良好，Vue.JS以及
React Native爆发，Yarn以及 Create React App新军突起。

我们已经讨论了2016年Github上面引领风尚的项目，但真正重要的是开发者的满意程度。所以，如果你想要更多干货，
去查一下Sacha Greif的JavaScript现状调查吧，收集了超过9000个回复！

----------

是时候来想想2017了，下一个JavaScript之星是谁？

这里是我挑选的10个项目，在2016年我很喜欢这些项目以及想法，2017年它们仍将蓬勃发展：
- Vue.JS：永不停歇
- Electron
- Create React App（用来创建页面）
- React Native
- Gatsby
- Yarn：一个快速、可信赖的安全依赖包，可替代npm，可以[在这儿](https://blog.risingstack.com/yarn-vs-npm-node-js-package-managers/)
查阅node.js包管理工具的状态
- “渐进式网页应用”概念
- Node.js的“微服务”解决方案，能够更好实现部署，例如Now
- Node.js的升级：最新一版已经实现对ES6语法很好的支持
- 最后一个就是GraphQL：我所有的朋友都告诉我，将有大事发生！



