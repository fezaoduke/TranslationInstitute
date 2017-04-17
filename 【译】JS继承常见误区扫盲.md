关于本文：

[原文地址](https://medium.com/javascript-scene/common-misconceptions-about-inheritance-in-javascript-d5d9bab29b0a)   [翻译地址](https://zhuanlan.zhihu.com/p/26252652)   译者：野草


什么鬼！！！当程序员遇到违反“最小惊讶原则”，违反他们直觉的时候，他们会不由自主地发出这句感叹！

举个栗子：

> .1 + .2 
0.30000000000000004

> 什么鬼！！！(*+﹏+*)~@

当我遇到一些资深前端开发，却不知道JavaScript原型继承(Prototypal inheritance)的时候，我也情不自禁地发出“什么鬼！！！”的感叹。原型继承可是计算机科学史上最重大的变革之一，也是JavaScript语言两大支柱之一。这对我来说就像，一位专业摄影师却还没学会曝光三角形(Exposure triangle)原理一样不可思议，这可是控制照片风格的基础啊。简而言之：

> 如果你不懂原型，那你根本就不懂JavaScript。


***问：类继承和原型继承不是同一回事儿吗，只是风格选择而已？***

***答：不是！***

类继承和原型继承不论从本质上还是从语法上来说，都是两个截然不同的概念。

二者之间有着区分彼此的本质性特征。要完全看懂本文，你必须牢牢记住以下几点：

类继承中，实例继承自模版（类），并且创建子类关系。换言之，你不能像使用实例一样使用类。实例由类创建出来，并且能调用类的方法，但是你不能直接在类上调用本身的方法。你必须创建一个实例，然后在实例上应用那些方法。

原型继承中，实例继承自其他的实例。它们使用的是原型委托（将实例的原型对象指向一个模板对象），这种方式被Kyle Simpson（你不知道的JS系列作者）称为对象关联（OLOO, Objects Linking to Other Objects）。使用这种关联继承，你只是将模板对象的属性拷贝到新的实例中而已。

理解上述区别至关重要。类继承的机制在创建子类的同时，也不小心创建了类的层级。

原型继承却可以避免创建类似的层级。建议原型链越短越好，其实很容易将很多原型扁平化为一个单委托原型。

总结：

+ 类是一个抽象的模版。

+ 原型是一个具体的对象实例。

***问：JavaScript中类不是创建对象的正确方式吗？***

***答：不是！***

JavaScript中创建对象有几种方式。最常见的一种是对象字面量方式。看个例子，用ES6语法写的对象：

    // ES6 或称 ES2015, 因为发布于2015.
    
    let mouse = {
      furColor: 'brown',
      legs: 4,
      tail: 'long, skinny',
      describe () {
        return `A mouse with ${this.furColor} fur,
          ${this.legs} legs, and a ${this.tail} tail.`;
      }
    };

当然，对象字面量方式比ES6出来早多了，但之前的写法缺少对象中函数方法的简写方式，以及定义变量时你只能用`var`而用不了`let`。对了，`describe()`方法中的模板字符串在ES5中也是不能用的。

我们可以利用ES5中的`Object.create()`附上对象的委托原型：

    let animal = {
      animalType: 'animal',
      
      describe () {
        return `An ${this.animalType}, with ${this.furColor} fur, 
          ${this.legs} legs, and a ${this.tail} tail.`;
      }
    };
    
    let mouse = Object.assign(Object.create(animal), {
      animalType: 'mouse',
      furColor: 'brown',
      legs: 4,
      tail: 'long, skinny'
    });

让我们仔细分析这个例子。`animal`是委托原型，`mouse`是实例。当你尝试获取`mouse`对象上没有的属性时，JavaScript将会在`animal`（委托对象）上寻找这个属性。

`Object.assign()`是ES6的新特性，由Rick Waldron提出。其实它早已在一些知名的库中被实现，比如jQuery中的`$.extend()`，Underscore中的`_.extend()`，还有Lodash中的`assign()`。该方法传入一个目标对象，以及任何多个用逗号隔开的源对象，它将会从最后一个源对象开始拷贝所有的可枚举属性到目标对象。若存在属性名冲突，前者会被后者覆盖。

Douglas Crockford提出了ES5中的`Object.create()`，它能使我们在不用构造器和`new`关键词的情况下，设置对象的委托原型。

本文不涉及到构造函数，因为我非常不推荐这种方式。太多滥用构造函数的例子，以及太多由此引起的麻烦。值得一提的是，很多聪明人并不同意我的看法。没关系，聪明人想怎么做就怎么做。

总有明智的人会听取Douglas Crockford的意见：

> 如果某个特性有时会不靠谱，而且存在一个更好的选择，那么还是选择那个更好的方式。

***问：难道不需要构造函数来定义实例的行为，以及进行实例化吗？***

***答：不需要！***
 
任何函数均可创建并返回对象。当该函数不是用作构造函数来创建时，它被称为工厂函数(factory function)。

**更佳选择**

    let animal = {
      animalType: 'animal',
     
      describe () {
        return `An ${this.animalType} with ${this.furColor} fur, 
          ${this.legs} legs, and a ${this.tail} tail.`;
      }
    };
     
    let mouseFactory = function mouseFactory () {
      return Object.assign(Object.create(animal), {
        animalType: 'mouse',
        furColor: 'brown',
        legs: 4,
        tail: 'long, skinny'
      });
    };
    
    let mickey = mouseFactory();

通常我不会将函数命名为`factory`，那只是一个形象的比喻。一般我就简单称之为`mouse()`。

***问：不需要用构造函数来创造私有变量或者属性吗？***

***答：不需要***

JavaScript中，当你返回一个函数，该函数可以访问外部函数的变量。当你使用这个函数的时候，JS引擎创建了一个闭包。闭包是JavaSript中非常常见的模式，它通常用来创建私有变量。

闭包并不是构造函数独有的。任何函数均可创建闭包：
    
    let animal = {
      animalType: 'animal',
     
      describe () {
        return `An ${this.animalType} with ${this.furColor} fur, 
          ${this.legs} legs, and a ${this.tail} tail.`;
      }
    };
     
    let mouseFactory = function mouseFactory () {
      let secret = 'secret agent';
    
      return Object.assign(Object.create(animal), {
        animalType: 'mouse',
        furColor: 'brown',
        legs: 4,
        tail: 'long, skinny',
        profession () {
          return secret;
        }
      });
    };
     
    let james = mouseFactory();
    

***问：使用`new`关键词是否意味着类继承？***

***答：不是！***

`new`关键词的作用是调用构造函数，具体做了以下几件事：

+ 创建一个新实例
+ 将`this`绑定于该实例
+ 将该实例的委托[[Prototype]]指向构造函数的`prototype`属性所指的对象
+ 以构造函数来命名对象属性，通常在debug阶段你会注意到获得实例对象的属性时，你会得到`[Object Foo]`而不是`[Object object]`。
+ 允许`instanceof`判断该实例的原型和构造函数的`prototype`属性是否指向同一个对象。

**不靠谱的`instanceof`**

是时候重新思考`instanceof`了，或许你开始质疑它的作用。

注意：`instanceof`并不是我们所预期的像强类型语言那样进行类型检查。它只是检查对象的原型属性，而且很容易忽悠别人或者被忽悠。比如，它不能在不同执行环境下起作用（比如iframe中），这也是出bug的常见原因之一。

另外，利用`instanceof`很可能得到错误的结果。因为它仅仅是对目标对象的`.prototype`属性的身份检查，所以可能会出现以下奇怪的现象：

    function foo(){}
    var bar = { a: ‘a’};
    foo.prototype = bar; // Object {a: “a”}
    baz = Object.create(bar); // Object {a: “a”}
    baz instanceof foo // true. oops.

最后一行的结果完全符合JavaScript对`instanceof`的定义。没有什么不对，仅仅是因为`instanceof`并不能保证结果的正确性罢了。它很容易得到错误的结果。

除此之外，强制代码强类型化，会让函数远离更有用的高复用度的类。

总而言之，`instanceof`限制了代码的可用性，也给程序带来了潜在的bug。

**奇怪的`new`**

什么？！`new`会返回一些奇怪的东西。如果你尝试返回一个基本数据类型，`new`做不到。倘若想返回其他任意对象，`new`可以做到，但这也意味着`this`被抛弃了，也就切断了所有能链接到`this`的引用（包括`.call()`和`.apply()`），同时返回东西和构造函数的`prototype`属性也没有了联系。

***问：类继承和原型继承性能差别大吗？***

***答：差别不大。***

你可能听说过[hidden classes](http://richardartoul.github.io/jekyll/update/2015/04/26/hidden-classes.html)，认为用构造函数来创建实例会比`Object.create()`快很多。其实，有点夸大其词。

项目运行中只有很微少的时间是用来运行脚本的，然后花在获取对象属性上的时间更是微乎其微。事实上，当今最慢的计算机每秒也可访问上百万个属性。

所以，这并不是项目性能优化的瓶颈。你需要做的是仔细分析项目，去发现真正的性能瓶颈。我相信在你思考这些非常微小的优化之前，有数不尽的地方值得你去优化。

不相信？若想该微优化明显提升性能，你必须成千上万次地循环涉及的操作，而且微优化中你唯一需要关心的地方是那些跟数量级相关的代码。

> 经验之谈：仔细分析你的项目，尽量减少网络加载，文件读写，渲染等可能的瓶颈。然后你才应该开始考虑微优化的问题。

你能区别.0000000001秒和.000000001秒吗？不能吧？我也不能，但我能区别加载10个小图标和加载一个字体的时间长短。

如果你真的分析了你的项目，并且发现瓶颈真的出在创建对象上，最快的解决方式不是用`new`或者类继承，而是使用对象字面量。如果因为性能你觉得值得放弃原型面向对象，那也值得同时放弃原型链和继承转而直接使用字面量对象。

> 可谷歌说使用类更快。。。

什么？！我没听错吧！谷歌做的是JavaScript引擎，而你做的是实际项目。显然你们二者关心的不是同一件事情。就让谷歌那小子去处理微优化的摊子。你就担心担心你自己应用真正的瓶颈。我敢说，你担心什么都比担心原型继承带来的性能问题好。
 
***问：类继承和原型继承内存消耗区别大吗？***

***答：不大！***

两者均可使用委托原型使实例共享方法，同时，它们也可使用或者避免将一堆状态变量封装到闭包里。

实际上，如果你用的是工厂函数，你能更容易操作这些对象，因为你会更加谨慎地处理内存问题，也能避免时不时被垃圾回收器阻碍。想了解更多有关构造函数的尴尬，请看“使用`new`关键词是否意味着类继承”问答中的最后一段。

简而言之，如果你想更随心地进行内存管理，请使用工厂函数，而非构造器或者类继承。
 

***问：原始API使用构造函数，难道不是因为它们比工厂模式更常用吗？***

***答：不是！***

JavaScript中工厂模式是极其常用的。比如，一直以来最流行的jQuery库，使用的也是工厂模式。John Resig选择工厂和原型扩展，而不是类。因为他可不想每次开发者进行DOM选择的时候，都要用`new`来初始化。简直不忍直视！


    /**
    以类为设计核心的jQuery - 非常糟糕，可能jQuery就从此被埋没！
    **/
     
    // 看起来有点蠢，我们是在创造一个id为"foo"的DOM元素吗？错，我们是在选择这个现有的元素。
    var $foo = new $('#foo');
    
    // 重复冗余的输入
    var $bar = new $('.bar');
    var $baz = new $('.baz');
    
    // 看下面这坨是什么鬼？ 
    var $bif = new $('.foo').on('click', function () {
      var $this = new $(this);
      $this.html('clicked!');
    });
    
 jQuery用工厂模式成功地避免了类似代码。
 
那还有哪些使用工厂模式的例子？
    
+ React中的`React.createClass()`是工厂函数；
+ Angular使用了类和工厂；
+ Ember中的`Ember.Application.create()` ；
+ Node中核心服务，如`http.createServer()`，`net.createServer()`；
+ Express也是一个工厂。

如上所见，几乎所有最流行的库和框架都使用了工厂函数。而JavaScript唯一比工厂还常见的对象实例化模式是对象字面量。

JavaScript内联函数使用的是构造函数，因为Brendan Eich设计语言的时候想让JavaScript设计得更像Java一点。考虑到自我连贯性，JavaScript就一直使用着构造函数的方式。现在想把所有东西都变成工厂，废弃构造函数就显得有点尴尬了。

> 但这并不意味着你的代码就要很糟糕。

结论是，工厂函数是个不错的选择。（译者话）

***问：难道类继承不比原型继承更常用吗？***

***答：不是！***
    
每当我听到这种误解，就想忍不住抛下一句：“你到底用过JavaScript没有？！”，然后走人。但，我还是忍住了，仔细跟对方澄清这个问题。

如果你也有这样的问题，不要沮丧。这不是你的问题，JavaScript培训TMD太差劲了！！！

答案是一个巨大的

错，但是。。。  
    
原型是JavaScript中的惯用继承模式，而“类”是外来入侵模式。

我们来回顾JavaScript库的简单发展史：

最初，开发者各自开发自己的库，也乐于开源出来。此时原型就随之出现了。原型利用链接继承(concatenative inheritance)来扩展内置的原型。

后来当网上充斥着大量的原生方法覆盖或者库冲突的时候，大家都意识到修改内置原型并不是一个好的模式。不过，那又是另一回事了。

后来最火的JavaScript库出来了——jQuery。而丰富的jQuery插件是jQuery极具吸引力的原因之一。它们通过链接继承来拓展jQuery的委托原型。

> 你有感觉到某种设计模式了吗？

至今jQuery一直是最受欢迎的JavaScript，远远超过其他的库。

然而这是JavaScript混乱的开始，用类拓展的方式开始侵入JavaScript。John Resig写了一篇文章[Simple Class Inheritance in JavaScript](http://ejohn.org/blog/simple-javascript-inheritance/)，开发者们纷纷开始效仿。即使John Resig本人也觉得jQuery不需要类，因为原型是更佳的选择。

接着，像ExtJS的类Java框架开始出现并且慢慢流行起来，开启了JavaScript类的非主流使用时代。那是2007年，JavaScript在12年之后因为一个流行库引入了类继承。

三年后，Backbone大爆发，其中`.extend()` 方法模仿类继承的实现，带来了很多糟糕的特性。这是JavaScript全面崩塌的开始。

> 曾经我们用Backbone开发一个百万行代码级别的应用。其中有个bug我花了几个月的时间，代码涉及6层继承关系。我沿着`super`链一行一行地检查每个构造函数的代码。最终在顶级类中找到bug。然后我又得修复子类中所有依赖这个错误的代码。本来几分钟能搞定的事，我花了几个小时才修复完成。太糟心了！

那代码根本不是JavaScript。我感觉我又回到了Java地狱，那种因为高耦合而带来的牵一发而动全身的恐惧感，无助感，孤独感迎面袭来。

这些也是重构最怕遇到的问题。

幸运的是，Backbone还有一片生机。

    var object = {};
    
    _.extend(object, Backbone.Events);
    
    object.on("alert", function(msg) {
      alert("Triggered " + msg);
    });
    
    object.trigger("alert", "an event"); 
    
`Backbone.Events` 混入的思想其实就是链接继承。

事实上，如果你仔细思考那些库，你会发现很多使用链接和委托的例子。这些例子太简单常见了，开发者根本没有意识到那就是继承。

> JavaScript继承非常简单，大家都认为它应该很难。于是为了达到我们的预期，我们引入了更复杂的类。

那我们是怎么引入类的呢？利用原型继承进行委托原型以及对象链接。这完全就是简单问题复杂化了。

***问：难道选择类继承还是原型继承不是决定于实际情况吗？***

***答：不是！***

数年来，我一直主张这样的观点：以原型为基础的面向对象更加简单，灵活，更不容易出错。很多人都听取了我的意见。我鼓励大家能举出一个使用类的例子来反驳我。我收到了一些回复，很不幸这些答案都是基于本文提到的一些误区。

曾经我也很喜欢用类继承，几乎在哪都用，代码中到处都是我创建的对象层级。为了帮助架构师设计对象层级关系，我还开发了一个可视化的快速应用开发工具。也即是说，在使用类继承的企业级应用中，我们需要一个可视化工具来协助我们理解和绘制对象层级关系。

在我从C++, Java转到JavaScript之后没多久，我不再这样做了。不是因为我开发的应用简单了（相反，我开发的更复杂了），而是因为JavaScript比C++, Java简单好用。从此，我再不没用过可视化工具来设计对象关系。

别人经常会咨询我软件设计的问题，我通常会建议他彻底重构。为什么呢？因为所有的对象层级在创建新的实例时总会出错。

我并不是一家之言。现如今，很多新版本开发都会彻底重构，其中大部分都是因为难维护的类层级而遗留下来的历史问题。几乎每个开发者都会有本[《设计模式》](https://www.amazon.com/gp/product/0201633612?ie=UTF8&camp=213733&creative=393185&creativeASIN=0201633612&linkCode=shr&tag=eejs-20&linkId=QYF6ABRMZ4O6KML2)，整本书都在讲述关系错误的设计模式，以及如何避免或者重构。

我还是劝告你，在这个问题上，你应该接纳四人帮（设计模式的作者）的意见：

> 优先使用对象组合，而非类继承。

Java中，这个比使用类继承还难，因为你需要用类来实现（Java所有对象都是通过类生成的）。

在JavaScript中，我们就没有这个借口了。事实上，比起管理对象层级，使用原型组合的方式来创建对象会简单很多。

什么？！

没骗你！如果你需要一个将日期变成`megaCalendarWidget`的jQuery对象，你不需要新开辟一个类。JavaScript能动态地拓展对象，只要拓展jQuery的原型对象就能做到。



    /*
    如何扩展jQuery原型对象？
    */
    
    jQuery.fn.megaCalendarWidget = megaCalendarWidget;
     
你再调用jQuery工厂函数，你会能得到上述你需要的实例。

同样，你也可以用`Object.assign()`来组合任意多个对象，以后者优先的顺序。

    import ninja from 'ninja'; // ES6 modules
    import mouse from 'mouse';
    
    let ninjamouse = Object.assign({}, mouse, ninja);

真的可以是任意数量的对象：

    // I'm not sure Object.assign() is available (ES6)
    // so this time I'll use Lodash. It's like Underscore,
    // with 200% more awesome. You could also use
    // jQuery.extend() or Underscore's _.extend()
    var assign = require('lodash/object/assign');
    
    var skydiving = require('skydiving');
    var ninja = require('ninja');
    var mouse = require('mouse');
    var wingsuit = require('wingsuit');
    
    // The amount of awesome in this next bit might be too much
    // for seniors with heart conditions or young children.
    var skydivingNinjaMouseWithWingsuit = assign({}, // create a new object
      skydiving, ninja, mouse, wingsuit); // copy all the awesome to it.

这就是我之前提到的链接继承，继承的那个原型有时会称为模版原型。区别于委托原型的委托属性，我们直接复制属性。

***问：ES6新增class关键词，我们不应该使用吗？***

***答：不应该！***

事实上，我们有一万个[避免使用ES6类](https://medium.com/javascript-scene/how-to-fix-the-es6-class-keyword-2d42bb3f4caf)的[理由](https://medium.com/javascript-scene/the-two-pillars-of-javascript-ee6f3281e7f3)，更别说类其实是JavaScript中一个尴尬的存在。

我们已经拥有了JavaScript非常强大富有表现力的对象系统。如今JavaScript中实现的类具有限制性（贬义词，不是指类型修正的限制），并且模糊了语言中内置的原型面向对象系统。

所以，你知道怎么用JavaScript比较好了吗？作为对原型面向对象设计非常熟悉的过来人告诉你，利用原型进行语法糖和抽象处理最好。


【译者话】文章到这里终于好了，这真的是又长又有深度又费解的文章。作者的观点比较另类前卫，反正我是远远没到这种觉悟的。我也无法肯定或者否定他的说法，希望这篇文章能带给你一些思考吧。最后附上这个作者的介绍，我就不翻译了，要吐了。。。

> Eric Elliott is the author of “Programming JavaScript Applications” (O’Reilly), & host of the documentary film-in-production, “Programming Literacy”. He has contributed to software experiences for Adobe Systems, Zumba Fitness, The Wall Street Journal, ESPN, BBC, and top recording artists including Usher, Frank Ocean, Metallica, and many more.

> He spends most of his time in the San Francisco Bay Area with the most beautiful woman in the world.
