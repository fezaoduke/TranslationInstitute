

关于本文：

[原文地址](https://www.sitepoint.com/writing-better-javascript-with-flow/)   [翻译地址](https://github.com/fezaoduke/TranslationInstitute/blob/master/%E7%94%A8Flow%E7%BC%96%E5%86%99%E6%9B%B4%E5%A5%BD%E7%9A%84JavaScript%E4%BB%A3%E7%A0%81.md) 译者：野草

本文发表于前端早读课[【第8**期】]()

你是否经常在debug那些简单可避免的bug？可能你给函数传参的时候搞错了参数的顺序，或者本来应该传个Number类型的参数，你传了一个String类型？JavaScript的弱类型是这一类bug的罪魁祸首，静态类型语言中不存在此类bug。

[Flow](https://flow.org/en/)就是JavaScript的静态类型检查工具，由Facebook团队于2014年的[Scale Conference](https://atscaleconference.com/)上首次提出。该库的目标在于检查JavaScript中的类型错误，开发者通常不需要修改代码即可使用，故使用成本很低。同时，它也提供额外语法支持，使得开发者能更大程度地发挥Flow的作用。

本文将介绍Flow及其主特性。下面分别从如何安装设置，如何添加类型注释，如何在运行时自动去掉注释等方面来介绍。

## 安装

目前Flow兼容的操作系统有Mac OS X，Linux(64位)，Windows(64位)。最简单的安装方式是通过npm：

    npm install --save-dev flow-bin 

然后在`package.json`文件中的`scripts`项添加：

    "scripts": {
      "flow": "flow"
    }
    
完成之后，就可以开始探索它的特性了。

## 启动

首先需要在项目的根目录下创建一个`.flowconfig`配置文件。我们可以通过以下命令创建一个空配置文件：

    npm run flow init
    
完成设置之后，在终端输入以下命令可以在你的项目根目录以及任何子目录文件夹下进行专门的类型检查：

    npm run flow check
    
但是，这并不是最高效的使用方式，因为每次Flow都会重新检查整个项目的所有文件。开发过程中，推荐启动Flow服务。

Flow服务的工作方式是增量检查，也就是说它只检查变化的部分。在终端输入以下命令来启动Flow服务：

    npm run flow
    
首次运行该命令时，服务启动并且显示最初类型检查结果。这保证了Flow更高效的增量式工作流。然后接下来每次想要知道检测结果，只要输入`flow`命令即可。开发结束之后，输入`npm run flow stop`停止服务。

Flow的类型检查是可选的，并不需要一次性检查所有代码。你可以选择你想要检查的文件，只要在对应的JavaScript文件最前面加上带有`@flow`标识的注释即可：

    /*@flow*/

当你想在已有项目中加入Flow的时候，该特性特别有帮助。因为你可以一一选择并检测你要的文件，然后修正错误。

## 类型推断

通常，类型检查分为以下两种方式：

+ **通过注释**：事先注释好我们期待的类型，Flow就会基于这些注释来评估
+ **通过代码推断**：通过变量的使用上下文来推断出变量类型，然后根据这些推断来检查类型

第一种方式，我们需要额外编写只在开发阶段起作用的代码，最后在代码编译打包的阶段被剔除。显然，这种额外添加类型注释的方式增加了工作量。

第二种方式，不需要任何代码修改即可进行类型检查，最小化开发者的工作量。它不会强制你改变开发习惯，因为它会自动推断出变量的类型。这就是所谓的**类型推断**，Flow最重要的特性之一。

我们来通过一个例子来说明这个特性：

    /*@flow*/

    function foo(x) {
      return x.split(' ');
    }
    
    foo(34);
    
当你在终端运行`npm run flow`命令的时候，上述代码会报错，因为函数`foo()`的期待参数是字符串，而我们输入了数字。

错误信息类似如下:

    index.js:4
      4:   return x.split(' ');
                    ^^^^^ property `split`. Property not found in
      4:   return x.split(' ');
                  ^ Number

上述信息清楚地指出了出错位置和错误原因。我们只要将参数变成字符串，即可修正错误，如下所示：

    /*@flow*/
    
    function foo(x) {
      return x.split(' ');
    };
    
    foo('Hello World!');

如上所述，以上代码不会报任何错。该例想说明的是，因为`split()`方法只适用于`string`类型的变量，所以`x`应该是`string`，这就是类型推断。

### 空类型

Flow处理`null`的方式与其他类型库不同。它不会忽略`null`，这样可以防止了因给变量传了`null`而导致程序崩溃的错误。

思考以下代码：

    /*@flow*/
    
    function stringLength (str) {
      return str.length;
    }
    
    var length = stringLength(null);
    
Flow会报错。为了防止出错，我们需要单独处理`null`。

    /*@flow*/
    
    function stringLength (str) {
      if (str !== null) {
        return str.length;
      }
    
      return 0;
    }
    
    var length = stringLength(null);
    
代码中我们引入对`null`的检查，确保代码能在任何情况下都正常且正确运行。上述代码可以通过Flow的类型检查。

## 类型注释

如上所述，类型推断是Flow最有用的特性之一，不需要编写类型注释就能获取有用的反馈。但在某些特定的场景下，添加类型注释可以提供更好更明确的检查依据。

考虑以下代码：

    /*@flow*/
    
    function foo(x, y){
      return x + y;
    }
    
    foo('Hello', 42);
    
Flow检查上述代码时检查不出任何错误，因为`+`即可以用在字符串上，也可以用在数字上，我们并没有明确指出`add()`的参数必须为数字。

在这种情况下，我们可以借助类型注释来指明期望的类型。类型注释是以冒号`:`开头，可以在函数参数，返回值，变量声明中使用。

如果我们在上段代码中添加类型注释，就会变成如下：
    
    /*@flow*/
    
    function foo(x : number, y : number) : number {
      return x + y;
    }
    
    foo('Hello', 42);
    
现在Flow就能检查出错误，因为函数参数的期待类型为数字，而我们提供了字符串。

Flow报错信息类似如下：

    index.js:7
      7: foo('Hello', 42);
             ^^^^^^^ string. This type is incompatible with the expected param type of
      3: function foo(x : number, y : number) : number{
                          ^^^^^^ number
                          
如果传入的参数是数字，就不会有错误。类型注释在大型复杂的JavaScript文件中也很有用，它能保证代码按照预期进行。

记住上一个例子，我们来看看Flow能支持的其他更多类型注释。

### 函数

    /*@flow*/
    
    /*--------- Type annotating a function --------*/
    function add(x : number, y : number) : number {
      return x + y;
    }
    
    add(3, 4);

上述代码展示了变量类型注释以及函数类型注释。函数`add()`的参数，以及函数的返回值，期待类型为数字。如果传入其他类型参数，Flow就会检测到错误。

### 数组

    /*-------- Type annotating an array ----------*/
    var foo : Array<number> = [1,2,3];

数组类型注释的格式是`Array<T>`，`T`表示数组中每项的数据类型。在上述代码中，`foo`是每项均为数字的数组。

### 类

 下面展示了类和对象的类型注释模型。唯一需要注意的是，可以在两个类型之间使用或逻辑，用`|`来间隔。变量`bar1`添加了必须为`Bar`类的类型注释。
 
     /*-------- Type annotating a Class ---------*/
    class Bar{
      x:string;           // x should be string       
      y:string | number;  // y can be either a string or a number
      constructor(x,y){
        this.x=x;
        this.y=y;
      }
    }
    
    var bar1 : Bar = new Bar("hello",4);

### 对象字面量

对象的类型注释类似于类，指定对象属性的类型。

    /*--------- Type annonating an object ---------*/
    
    var obj : {a : string, b : number, c: Array<string>, d : Bar} = {
      a : "hello",
      b : 42,
      c : ["hello", "world"],
      d : new Bar("hello",3)
    }

### Null

若想任意类型`T`可以为`null`或者`undefined`，只需类似如下写成`?T`的格式即可。

    /*@flow*/
    
    var foo : ?string = null;

此时，`foo`可以为字符串，也可以为`null`。

目前我们只对Flow的类型注释做了很浅的探索。一旦你习惯了使用这些基本类型，建议在Flow官网上的[类型文档](https://flow.org/en/docs/types/)深入了解所有的类型。

## 库定义

我们经常需要引入第三方库，Flow检查时就会抛出错误。但这并不是我们期待的错误。

庆幸的是，我们不需要修改库源码去防止这些报错。我们只需创建一个库定义（libdef）。libdef是包含第三方库声明的JS文件简称。

观察下面的例子：

    /* @flow */
    
    var users = [
      { name: 'John', designation: 'developer' },
      { name: 'Doe', designation: 'designer' }
    ];
    
    function getDeveloper() {
      return _.findWhere(users, {designation: 'developer'});
    }

Flow会检查出以下错误：

    interfaces/app.js:9
      9:   return _.findWhere(users, {designation: 'developer'});
                  ^ identifier `_`. Could not resolve name
                 
由于Flow并不认识`_`，所以会报错。要解决这个问题，我们需要引入Underscore的库定义。

### 使用flow-typed

flow-typed仓库包含了众多流行的第三方库的libdef。只需在项目根目录下创建一个名为`flow-typed`的文件夹，并且下载相关的定义文件即可。

为了进一步简化，可以用npm的命令行方式一键获取和安装libdef文件：

    npm install -g flow-typed
    
安装成功之后， 运行`flow-typed install`来检查`package.json`文件，并且下载所有项目中用到的第三方库的libdef。

### 自定义libdef

如果你用的库并不在flow-typed仓库，你可以创建你自己的libdef。本文不会细谈自定义libdef，因为很少会有人遇到，感兴趣可以查看[此文档](https://flow.org/en/docs/libdefs/creation/)。

## 剔除类型注释

由于额外添加的类型注释不是正确的JavaScript语法，打包编译的时候需要在源码中剔除。可以通过[flow-remove-types](https://github.com/flowtype/flow-remove-types)来剔除，或者如果你已经用Babel来转译JS，你可以使用[Babel preset](https://babeljs.io/docs/plugins/preset-flow/)来移除。我们只讨论第一种方法。

首先需要安装flow-remove-types作为项目依赖库：

    npm install --save-dev flow-remove-types
    
然后在`package.json`文件中添加另一个`script`入口：

    "scripts": {
      "flow": "flow",
      "build": "flow-remove-types src/ -D dest/",
    }

上述命令将剔除`src`文件夹下的所有类型注释，在`dist`文件夹中保存编译后的版本。编译后的文件就是普通的能运行于浏览器的JavaScript文件。
 
 ## 结语
 
 本文讨论了Flow各种各样的类型检查特性，展示了Flow如何帮助我们捕获错误提高代码质量。我们也看到了如何用可选的方式去逐个检查JS文件，如何做类型推断。
 
 你觉得Flow这种对JavaScript进行静态类型检查的方式如何？颇有用处，还是画蛇添足？看了本文后，是不是跃跃欲试了？欢迎分享你的想法，说说你的疑问。 
 
 
