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

##### 2016年的王者是...
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




