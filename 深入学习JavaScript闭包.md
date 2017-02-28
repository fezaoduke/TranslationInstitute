关于本文：

[原文地址]( https://medium.freecodecamp.com/lets-learn-javascript-closures-66feb44f6a44?gi=e0c9ec232139#.23peoxdhn)  [翻译地址](**********)   译者：野草 

本文发表于前端早读课[【第8****期】](***********)


闭包（closure）是JavaScript中最基本的概念，也是每个前端工程师都应该彻底掌握的概念。网上有很多教程，通俗易懂地解释了闭包的概念，却鲜有深入探讨闭包背后的机制和原理。

笔者在工作中深有感悟，彻底地理解和掌握某个知识点的本质，会让你在工作中游刃有余。因此经过笔者深入地学习调研之后，本文全面而透彻地分析闭包的运行方式，以及运行背后的机制。

希望读完本文之后，你能有所收获，并能在以后的工作中利用好闭包，发挥它最大的作用。

## 闭包是什么

闭包是JavaScript（包括其他绝大多数语言）中非常强大的特性，[MDN]( https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)上是这样定义的：

> 闭包是指那些能够访问自由变量（既不是本地定义而不作为参数的那些变量）的函数。换句话说，这些函数可以“记住”它被创建时候的环境。

我们来看些例子。 

### Example 1： 

	function numberGenerator() {
	  // 闭包内的局部自由变量
	  var num = 1;
	  function checkNumber() { 
	    console.log(num);
	  }
	  num++;
	  return checkNumber;
	}
	
	var number = numberGenerator();
	number(); // 2

本例中，函数`numberGenerator`创建了局部自由变量`num`，以及打印`num`的函数` checkNumber `。` checkNumber `没有自己的局部变量，却可以访问`numberGenerator`内的变量，这就是闭包在起作用。因此，即使在`numberGenerator`执行完毕之后，` checkNumber `仍然能成功访问`num`，并打印出来。

### Example 2：

我们用本例来证明，闭包能访问外部封闭函数内定义的所有变量。
	
	function sayHello() {
	  var say = function() { console.log(hello); }
	  // 闭包内的局部自由变量
	  var hello = 'Hello, world!';
	  return say;
	}
	var sayHelloClosure = sayHello(); 
	sayHelloClosure(); // ‘Hello, world!’

我们发现，变量`hello`是在匿名函数之后定义，但它仍能被匿名函数访问到。因为程序在编译的时候，变量`hello`已经在函数作用域中定义，程序执行的时候自然就能访问到了。稍后我会解释作用域是什么，没看懂可以暂时跳过。

### 闭包总揽

上述例子从比较高的层次去阐释了闭包的概念，归纳为一句话：“定义在封闭函数（enclosing function）中的变量，即使在该封闭函数执行完之后，仍然能被访问到”。显然，这不合常理的现象背后肯定有些不寻常的东西。

为了彻底搞清楚闭包，我们需要“掘地三尺”，深挖与闭包相关的概念。准备好了吗？那我们就从最核心的“执行上下文”（Execution Context，函数运行的环境）开始说起吧。

## 执行上下文 

执行上下文是ECMAScript标准中定义的一个抽象概念，用来记录代码运行的环境。它可以是代码最开始执行的全局上下文，也可以是执行进入某个函数体内的上下文。

![执行上下文分析](http://img.blog.csdn.net/20170228211825957?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZmVuNzQ3MDQyNzk2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

需要注意的是，程序至始至终只能进入某一个执行上下文。这也是为什么说JavaScript是单线程的原因，即每次只能有一个命令在执行。通常，浏览器用“栈”来维护执行上下文。“栈”遵循“后入先出（Last In First Out）”的原则，也就是说最后进栈的最先出栈（因为我们只能操作栈顶）。当前起作用的执行上下文位于栈顶，当它内部的代码执行完毕之后出栈 ，然后将下一个元素作为当前的上下文。

另外，程序并不需要执行完当前的执行上下文中的所有代码，才能进入另一个执行上下文。经常会有当前的上下文A执行到一半暂停，又进行了另一个上下文B的情况。当程序在上下文B中彻底结束之后，又回到上下文A中，从它暂停的地方重新继续执行。每次一个上下文被另外一个上下文所替代时，这个新的上下文就入栈成为栈顶，即当前的上下文。

![执行上下文栈](http://img.blog.csdn.net/20170228211942746?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZmVuNzQ3MDQyNzk2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

举个比较实际的例子。

	var x = 10;
	function foo(a) {
	  var b = 20;
	
	  function bar(c) {
	    var d = 30;
	    return boop(x + a + b + c + d);
	  }
	
	  function boop(e) {
	    return e * -1;
	  }
	
	  return bar;
	}
	
	var moar = foo(5); // 闭包 
	// moar函数实际上是foo函数执行之后返回的bar函数。
	// bar函数中调用boop，此时bar函数执行暂停
	// boop函数进入了执行，而boop对应的执行上下文就成了执行上下文栈中的栈顶。  
	moar(15); 

![这里写图片描述](http://img.blog.csdn.net/20170228212042794?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZmVuNzQ3MDQyNzk2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

当boop函数执行完毕，boop函数的执行上下文出栈，bar函数又继续执行。

![这里写图片描述](http://img.blog.csdn.net/20170228212059857?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZmVuNzQ3MDQyNzk2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

当有一堆执行上下文，一个接着一个地运行，有些执行到一半暂停后来又继续，这里需要回来继续执行的时候能记住当前的状态。事实上，ECMAScript中已经做出了规定，每个执行上下文都有用来追踪执行过程的各种状态组件。它们包括以下几个：

+ 代码执行状态（Code evaluation state）: 在当前执行上下文中用来记录代码执行，暂停，重新执行的状态
+ 函数（Function）：当前上下文正在执行的函数体（如果当前上下文是脚本或者模块，函数则为null）
+ 范畴（Realm）：内部对象集合，全局运行环境及其作用于下的所有代码，其他相关的状态、资源
+ 词法环境（Lexical Environment）：用来解决当前上下文中的标识符引用问题
+ 变量环境（Variable Environment）：词法环境下与环境记录（EnvironmentRecord）紧密联系的变量声明（VariableStatements）

是不是都看晕了？（译者也有译晕了！）没关系，这里我们只关心词法环境，它明确地解释了标识符引用的问题。你可以认为标识符就是变量。我们最初的目的是为了搞清楚函数在返回之后内部的变量为何还能被访问到，词法环境正是我们需要深挖的东西。

Note： 严格上来说，环境变量和词法变量都与闭包的实现有关系。但为简单起见，我们把它们合并称为“环境”。若想详细了解二者的区别，请看Dr. Alex Rauschmayer写的[文章]( http://www.2ality.com/2011/04/ecmascript-5-spec-lexicalenvironment.html)。

## 词法环境

[ES6]( http://www.ecma-international.org/ecma-262/6.0/#sec-lexical-environments)中明确定义：词法环境是用来定义标识符和具体变量之间的关系以及词法嵌套结构内的函数，它包括环境记录和指向外部词法环境的引用（有可能指向null）。通常词法环境跟一些具体语法结构有关，比如函数声明（FunctionDeclaration），块语句声明（BlockStatement），Try/Catch语句，这些代码在执行的时候又会生成一个新的词法环境。

我们来仔细解读一下。

+ 用来定义标识符的值：词法环境的目的就是管理代码中的数据。也就是说，它给标识符赋值，让标识符变得有意义。比如，代码段“console.log(x/10)”，如果x没有具体值，x是没有意义的，这段代码也没有意义。词法环境通过环境记录将标识符和具体的值联系在一起（见下一点）。

+ 词法环境包含环境记录：环境记录完美地记录了词法环境中所有标识符和具体值之间的联系，并且每个词法环境都有自己的环境记录。

+ 词法嵌套结构：内部环境引用包含它的外部环境，外部环境还可以有自己的外部环境。因此，一个环境可以作为多个内部环境的外部环境。全局环境是唯一一个没有外部环境的环境。说起来有点绕，我们用洋葱来做个比喻：全局环境是洋葱最外面的一层，每一层都嵌套在里面，一层又一层，就像我们的词法环境。

![这里写图片描述](http://img.blog.csdn.net/20170228212923197?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZmVuNzQ3MDQyNzk2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

我们用伪代码来抽象一下：

	LexicalEnvironment = {
	  EnvironmentRecord: {
	  // 标识符的赋值操作
	  },
          //外部环境的引用 
	  outer: < >
	};

+ 这些代码在执行的时候又会生成一个新的词法环境：每当外部封闭函数执行时，就会产生一个新的词法环境。这很重要，最后会回来讲这点。（函数不是唯一创建词法环境的方式，块语句，catch语句都可以。还是为了简单，我们只关注由函数创建的环境。）

总而言之，每个执行上下文都对应一个词法环境。这个词法环境中记录着变量和它对应的值，还有指向外部环境的引用。它可以是全局环境，模块环境（包括模块之间的引用关系），或函数环境（由函数调用而创建的环境）。

## 作用域链（Scope Chain）

基于上述定义，我们已经知道一个环境可以访问它的外部环境，它的外部环境又可以继续访问它外部的外部环境，以此类推。每个环境能访问到的标识符集合，我们称之为“作用域”。我们将作用域一层一层嵌套，形成了“作用域链”。

我们来看嵌套结构的例子。

  	var x = 10;
	
	function foo() {
	  var y = 20; // free variable
	  function bar() {
	    var z = 15; // free variable
	    return x + y + z;
	  }
	  return bar;
	}

如上所示，bar函数嵌套在foo函数中。我们用下面这张图来更直观地感受函数的嵌套关系。

![这里写图片描述](http://img.blog.csdn.net/20170228212152646?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZmVuNzQ3MDQyNzk2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

等下我们还会回到这个例子。

这个作用域链，或者说函数的环境链，在函数创建的时候就保存起来了。也就是说，它是由源代码的位置静态定义的，（这就是我们熟悉的词法作用域Lexical Scope）

我们先快速地了解一下，“动态作用域”（Dynamic Scope）与“静态作用域”（Static Scope）的区别，我们就能知道为什么静态作用域是产生闭包的一个必要条件了。

## 题外话：动态作用域 vs. 静态作用域

动态作用域的实现是基于栈结构，局部变量和函数参数都是存在栈里。因此，变量的具体值是由运行时当前的栈顶的执行上下文决定的。而静态作用域是指变量在创建的时候就决定了它的值，也就是说，源代码的位置决定了变量的值。你可能对二者的区别还是有点模糊，我们来看两个例子帮助我们理解。

### Example 1： 

	var x = 10;
	
	function foo() {
	  var y = x + 5;
	  return y;
	}
	 
	function bar() {
	  var x = 2;
	  return foo();
	}
	 
	function main() {
	  foo(); // 静态作用域: 15; 动态作用域: 15
	  bar(); // 静态作用域: 15; 动态作用域: 7
	  return 0;
	}

上例中，`bar`函数本质上就是执行`foo`函数，如果是静态作用域的话，`bar`函数中的变量`x`是在foo函数创建的时候就确定了，也就是说变量`x`一直为10，两次输出应该是相同的。而动态作用域则根据运行时的`x`值而返回不同的结果。当执行`bar`函数时，`x`的值变成了2，那么`foo`函数执行后的结果就是7。（你猜实际上输出是什么呢？）

### Example 2： 

	var myVar = 100;
	 
	function foo() {
	  console.log(myVar);
	}
	 
	foo(); //静态作用域: 100; 动态作用域: 100
	 
	(function () {
	  var myVar = 50;
	  foo(); // 静态作用域: 100; 动态作用域: 50
	})();
	
	// 高级函数
	(function (arg) {
	  var myVar = 1500;
	  arg();  // 静态作用域: 100; 动态作用域: 1500
	})(foo);

类似地，上例中`foo`函数的`myVar`变量在代码创建后，就已经确认了。也就是说，静态作用域使得程序在两个立即执行函数创建的时候将`myVar`变量分配好了。

动态作用域经常会带来不确定性，它不能确定变量的值到底是来自哪个作用域的。

## 闭包

上面讲了那么多，看起来有点离题，但其实是覆盖了理解闭包所需的全部知识点。

> 每个函数都有一个执行上下文，它包含了一个指定函数内变量赋值以及对外部环境的引用的词法环境。对外部环境的引用使得所有的内部函数能访问到外部作用域的所有变量，无论这些内部函数是在它创建时的作用域内调用还是作用域外调用。

看上去函数“记住”了外部环境，但其实上是这个函数有个指向外部环境的引用。

让我们重新回到嵌套结构的例子：

	var x = 10;
	
	function foo() {
	  var y = 20; // 自由变量
	  function bar() {
	    var z = 15; //自由变量
	    return x + y + z;
	  }
	  return bar;
	}
	
	var test = foo();
	
	test(); // 45

基于我们对环境运行机制的理解，上述例子的环境定义可以抽象为如下（伪代码）：

	GlobalEnvironment = {
	  EnvironmentRecord: { 
	    // 内置标识符
	    Array: '<func>',
	    Object: '<func>',
	    // 等等..
	    
	    // 自定义标识符
	    x: 10
	  },
	  outer: null
	};
	 
	fooEnvironment = {
	  EnvironmentRecord: {
	    y: 20,
	    bar: '<func>'
	  }
	  outer: GlobalEnvironment
	};
	
	barEnvironment = {
	  EnvironmentRecord: {
	    z: 15
	  }
	  outer: fooEnvironment
	};

当我们执行`test`函数时，我们会得到45。因为`test`函数执行了`foo`函数，而`foo`函数返回了`bar`函数。而`bar`函数即使在`foo`函数返回之后，仍然能访问到自由变量`y`，因为它有指向外部环境（也就是`foo`函数所在的环境）的引用。同理，`bar`函数也能访问到全局变量`x`，因为`foo`函数所在的环境能访问全局环境。这就是所谓的“作用域链查找”。

回到我们刚才讨论的动态作用域和静态作用域所论述的那点，为了实现闭包，我们不能用动态作用域的动态堆栈来存储变量。如果是这样，当函数返回时，变量就必须出栈，而不再存在，这与最初闭包的定义是矛盾的。事实上，外部环境的闭包数据被存在了“堆”中，这样才使得即使函数返回之后内部的变量仍然一直存在（即使它的执行上下文也已经出栈）。

好，现在我们已经从抽象意义上明白闭包的原理。我们再来看一下例子。

### Example 1： 

一个典型的错误是在for循环中有个函数，并且这个函数又用到计数变量。

	var result = [];
	 
	for (var i = 0; i < 5; i++) {
	  result[i] = function () {
	    console.log(i);
	  };
	}
	
	result[0](); // 5, 期待输出0
	result[1](); // 5, 期待输出1
	result[2](); // 5, 期待输出2
	result[3](); // 5, 期待输出3
	result[4](); // 5, 期待输出4

根据我们刚才所学的，很容易发现这个例子中的错误吧。对环境进行抽象分析，当for循环结束之后，环境是这样的：
```
environment: {
  EnvironmentRecord: {
    result: [...],
    i: 5
  },
  outer: null,
}
```
你以为`result`数组的五个函数的作用域是不同的，实际上它们是在同一个作用域下。因此，每次变量`i`增加时，整个作用域下的`i`也是跟着增加的，而这个`i`是被所有的函数共享的。因为for循环之后，`i`变成了5，所以`result`数组的函数执行结果都是5。

解决的方法之一是为每个函数创建一个额外的封闭上下文，这样它们就有了各自的执行上下文。


	 
	Var result = [];
	for (var i = 0; i < 5; i++) {
	  result[i] = (function inner(x) {
	    // 额外的封闭函数
	    return function() {
	      console.log(x);
	    }
	  })(i);
	}
	
	result[0](); // 0, 期待输出0
	result[1](); // 1, 期待输出1
	result[2](); // 2, 期待输出2
	result[3](); // 3, 期待输出3
	result[4](); // 4, 期待输出4

搞定啦！！！

另外一种机智的解决方法是用`let`声明变量，因为`let`能创建一个新的作用域，也就是说for循环的每次迭代中都新创建了一个标识符`i`。
	
	var result = [];
	for (let i = 0; i < 5; i++) {
	  result[i] = function () {
	    console.log(i);
	  };
	}
	
	result[0](); // 0, 期待输出 0
	result[1](); // 1, 期待输出1
	result[2](); // 2, 期待输出2
	result[3](); // 3, 期待输出3
	result[4](); // 4, 期待输出4


### Example 2：

本例可以看到每次函数的调用是如何生成一个新的闭包。

```
function iCantThinkOfAName(num, obj) { 
  //array变量和两个函数参数，被嵌套函数doSomething捕获
  var array = [1, 2, 3];
  function doSomething(i) {
    num += i;
    array.push(num);
    console.log('num: ' + num);
    console.log('array: ' + array);
    console.log('obj.value: ' + obj.value);
  }
  
  return doSomething;
}

var referenceObject = { value: 10 };
var foo = iCantThinkOfAName(2, referenceObject); // 闭包 #1
var bar = iCantThinkOfAName(6, referenceObject); // 闭包 #2

foo(2); 
/*
  num: 4
  array: 1,2,3,4
  obj.value: 10
*/

bar(2); 
/*
  num: 8
  array: 1,2,3,8
  obj.value: 10
*/

referenceObject.value++;

foo(4);
/*
  num: 8
  array: 1,2,3,4,8
  obj.value: 11
*/

bar(4); 
/*
  num: 12
  array: 1,2,3,8,12
  obj.value: 11
*/
```

函数`iCantThinkOfAName`每次调用都生成了一个新的闭包，也就是函数`foo`和函数`bar`。随后闭包函数的调用更新了闭包内的变量，这也证明了即使当函数`iCantThinkOfAName`返回之后，函数`iCantThinkOfAName`中的函数`doSomething`还是能访问闭包内的变量`array`，`obj`
和`num`。

### Example 3：

```
function mysteriousCalculator(a, b) {
	var mysteriousVariable = 3;
	return {
		add: function() {
			var result = a + b + mysteriousVariable;
			return toFixedTwoPlaces(result);
		},
		
		subtract: function() {
			var result = a - b - mysteriousVariable;
			return toFixedTwoPlaces(result);
		}
	}
}

function toFixedTwoPlaces(value) {
	return value.toFixed(2);
}

var myCalculator = mysteriousCalculator(10.01, 2.01);
myCalculator.add() // 15.02
myCalculator.subtract() // 5.00
```

我们可以看到函数`mysteriousCalculator `在全局作用域下，它返回两个函数。本例中的环境抽象出来是这样的：
```
GlobalEnvironment = {
  EnvironmentRecord: { 
    // 内置标识符
    Array: '<func>',
    Object: '<func>',
    // 等等...

    //自定义标识符
    mysteriousCalculator: '<func>',
    toFixedTwoPlaces: '<func>',
  },
  outer: null,
};
 
mysteriousCalculatorEnvironment = {
  EnvironmentRecord: {
    a: 10.01,
    b: 2.01,  
    mysteriousVariable: 3,
  }
  outer: GlobalEnvironment,
};

addEnvironment = {
  EnvironmentRecord: {
    result: 15.02
  }
  outer: mysteriousCalculatorEnvironment,
};

subtractEnvironment = {
  EnvironmentRecord: {
    result: 5.00
  }
  outer: mysteriousCalculatorEnvironment,
};
```
由于函数`add`和`subtract`有一个指向外部函数环境`mysteriousCalculator `的引用，它们能访问到该环境中的变量`mysteriousVariable `，并且得出最后的计算结果。

### Example 4：

最后一个例子，我们用来展示闭包的一个重要作用：在外部作用域下维护一个私有变量。

	function secretPassword() {
	  var password = 'xh38sk';
	  return {
	    guessPassword: function(guess) {
	      if (guess === password) {
	        return true;
	      } else {
	        return false;
	      }
	    }
	  }
	}
	
	var passwordGame = secretPassword();
	passwordGame.guessPassword('heyisthisit?'); // false
	passwordGame.guessPassword('xh38sk'); // true

这个作用非常强大。它让变量`password`成了闭包函数`guessPassword `的私有变量，只能被函数`guessPassword `访问，而不能被所有的外部函数访问。

## 总结

+ 执行上下文是ECMAScript标准中定义的一个抽象概念，用来记录代码运行的环境。程序至始至终只能进入一个执行上下文。
+ 每个执行上下文都有一个词法环境，它包含了标识符的赋值以及指向外部环境的引用。
+ 每个环境能访问到的标识符集合，我们称之为“作用域”。我们将作用域一层一层嵌套，形成了“作用域链”。
+ 每个函数都有一个执行上下文，它包含了一个指定函数内变量赋值以及对外部环境的引用的词法环境。看上去函数“记住”了外部环境，但其实上是这个函数有个指向外部环境的引用。这就是“闭包”的概念。
+ 每当外部封闭函数执行的时候就产生了闭包，也就是说闭包的创建并不一定需要内部函数返回。

+ JavaScript中闭包作用域是词法作用域，即它在代码写好之后就被静态决定了它的作用域。

+ 闭包有很多实际用处。其中一个重要的用处就是在外部作用域下维护一个私有变量。

## 结束语

希望本文能对你有帮助，现在你能完全理解JavaScript中的闭包是如何实现了吗？彻彻底底地掌握闭包的工作原理之后，你能更容易发现闭包的存在，更不用说在debug的时候省了不少力。

P.S. 若有不当之处，欢迎指正。

### 延伸阅读

简洁起见，我省去了一些可能读者朋友感兴趣的话题，链接如下：

+ 执行上下文中的变量环境具体是指什么？Dr. Axel Rauschmayer做了很好的解释，请戳[这里](http://www.2ality.com/2011/04/ecmascript-5-spec-lexicalenvironment.html)
+ 环境记录中有哪些不同的类型？请看[这里](http://www.ecma-international.org/ecma-262/6.0/#sec-environment-records)
+ MDN上关于[闭包]( https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)的定义

