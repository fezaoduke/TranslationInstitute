> 原文链接：https://medium.freecodecamp.com/understanding-node-js-event-driven-architecture-223292fcbc2d 
> 
> 作者：Samer Buna
>
> 翻译：野草

Node中的绝大多数对象，比如HTTP请求，响应，流，都是实现了`EventEmitter`模块，所以它们可以触发或监听事件。

![](https://cdn-images-1.medium.com/max/2000/1*74K5OhiYt7WTR0WuVGeNLQ.png
)

能体现事件驱动机制本质的最简单形式就是函数的回调，比如Node中常用的`fs.readFile`。在这个例子中，事件仅触发一次（当Node完成文件的读取操作后），回调函数也就充当了事件处理者的身份。

让我们更深入地探究一下回调形式。

## Node的回调 

Node处理异步事件最开始使用的是回调。很久之后（也就是现在），原生JavaScript有了Promise对象和async／await特性来处理异步。

回调函数其实就是作为函数参数的函数，这个概念的实现得益于JavaScript语言中的函数是第一类对象。

但我们必须要搞清楚，回调并不意味着异步。函数的回调可以是同步的，也可以是异步的。

比如，下例中的主函数`fileSize`接受一个名为`cb`的回调函数。该回调函数可以根据判断条件来决定是同步执行还是异步执行回调。

    function fileSize (fileName, cb) {
      if (typeof fileName !== 'string') {
        return cb(new TypeError('argument should be string')); // 同步调用
      }
      fs.stat(fileName, (err, stats) => {
        if (err) { return cb(err); } // 异步调用
        cb(null, stats.size);        // 异步调用
      });
    }
    
注意，这是不好的实践，很容易出现意想不到的bug。设计主函数时，回调函数的调用应该总是同步或者异步的。

再看一个经典的异步回调例子：

    const readFileAsArray = function(file, cb) {
      fs.readFile(file, function(err, data) {
        if (err) {
          return cb(err);
        }
        const lines = data.toString().trim().split('\n');
        cb(null, lines);
      });
    };
    
`readFileAsArray`接收一个文件路径参数以及一个回调函数。它读取文件内容，将内容拆分成数组`lines`，然后调用回调函数处理这个数组。

举个实例。假设有个`numbers.txt`文件，内容如下：

    //numbers.txt
    10
    11
    12
    13
    14
    15
    
现需要计算文件中奇数的个数，上面的`readFileAsArray`函数就可以利用起来了：

    readFileAsArray('./numbers.txt', (err, lines) => {
      if (err) throw err;
      
      const numbers = lines.map(Number);
      const oddNumbers = numbers.filter(n => n%2 === 1);
      console.log('Odd numbers count:', oddNumbers.length);
    });
	
这段代码读取txt文件中的数字成字符数组，解析成数字，然后计算出奇数的个数。

此处的回调函数用得恰到好处。主函数将回调函数作为最后一个参数，而回调函数的第一个参数是可为`null`的错误信息参数`err`。这种参数传递方式是开发者默认的规则，你最好也遵守：将回调作为主函数的最后一个参数，将错误信息作为回调函数的第一个参数。

## Promise：回调的取代者

如今，JavaScript有了Promise对象，异步可以不再需要回调了。回调方式将回调函数作为参数传递给主函数，同时在主函数内部处理错误信息。Promise对象则不同，它可以单独处理成功／失败情况，也可以链接多个异步调用，而不是嵌套处理。

如果`readFileAsArray`函数支持Promise写法，我们就可以这么用：

    readFileAsArray('./numbers.txt')
    .then(lines => {
        const numbers = lines.map(Number);
        const oddNumbers = numbers.filter(n => n%2 === 1);
        console.log('Odd numbers count:', oddNumbers.length);
    })
    .catch(console.error);

Promise用法使得我们可以直接在主函数的返回值上调用`.then`函数，而不是传入一个回调函数。`.then`函数能获取到之前用回调获取的内容，并且执行相同的业务操作。继续添加`.catch`函数，捕捉可能会产生的错误信息。

由于原生JavaScript自带 Promise对象，主函数很容易改造成支持Promise接口。以下是改造后的结合回调方式的`readFileAsArray`。

    const readFileAsArray = function(file, cb = () => {}) {
      return new Promise((resolve, reject) => {
        fs.readFile(file, function(err, data) {
          if (err) {
            reject(err);
            return cb(err);
          }
          const lines = data.toString().trim().split('\n');
          resolve(lines);
          cb(null, lines);
        });
      });
    };
    
现在这个函数返回一个包含`fs.readFile`异步调用的Promise对象。Promise对象有两个参数，`resolve`函数和`reject`函数。

当我们获取了错误信息需要回调时，用`reject`处理信息；反之，当我们获取结果数据需要回调时，用`resolve`来处理。

另外，回调函数要指定一个缺省值，以免直接用Promise接口调用，这里我们指定为空函数`()=>{}`。

## Promise升级：结合async/await使用

当异步遇到循环的时候，Promise接口会让代码简单很多。用回调的话，代码容易混乱。处理异步的最新特性是`async`函数，它能让我们像处理同步函数一样处理异步函数，使得代码更具可读性。

我们用async/await的方式调用`readFileAsArray`:

    async function countOdd () {
      try {
        const lines = await readFileAsArray('./numbers');
        const numbers = lines.map(Number);
        const oddCount = numbers.filter(n => n%2 === 1).length;
        console.log('Odd numbers count:', oddCount);
      } catch(err) {
        console.error(err);
      }
    }
    countOdd();

首先创建一个异步函数，其实就是一个带`async`关键词的普通函数。函数内部，在`readFileAsArray`函数前面加上`await`关键词，保证lines结果返回才执行下一行。 

执行这个异步函数`countOdd`，就能得到我们想要的结果。代码看起来简单且更具可读性。需要注意的是，我们需要用`try`/`catch`处理这个异步调用，以免出错。

有了async/await特性之后，我们不再需要像`.then`,`.catch`之类的特殊接口。我们仅仅标记一下函数，然后用纯原生的代码写书。

我们可以给所有支持Promise接口的函数添加async/await特性，不过，不包括异步回调的函数，比如setTimeout。

## EventEmitter模块

EventEmitter是促进Node中对象之间交流的模块，它是Node异步事件驱动机制的核心。Node中很多自带的模块都继承自事件触发模块。

概念很简单：触发器触发事件，该事件对应的监听函数被调用。也就是说，触发器有两个特征：

+ 触发某个事件
+ 注册／注销监听函数

我们创建一个继承EventEmitter模块的类：

    class MyEmitter extends EventEmitter {
    
    }
    
实例化该类，得到一个事件触发器：
    
    const myEmitter = new MyEmitter()；

在事件触发器的生命周期任何时候，我们都能利用emit函数触发已有的事件。

    myEmitter.emit('something-happened');

触发事件意味着某些情况发生，通常是关于触发器的状态变化。

使用`on`方法添加某个事件的监听函数，每次触发器触发事件时，对应的监听函数就会被执行。

## 事件!==异步

看个例子：

    const EventEmitter = require('events');
    
    class WithLog extends EventEmitter {
      execute(taskFunc) {
        console.log('Before executing');
        this.emit('begin');
        taskFunc();
        this.emit('end');
        console.log('After executing');
      }
    }
    
    const withLog = new WithLog();
    
    withLog.on('begin', () => console.log('About to execute'));
    withLog.on('end', () => console.log('Done with execute'));
    
    withLog.execute(() => console.log('*** Executing task ***'));

`WithLog`类是事件触发器，它里面定义了`execute`函数。该函数接收一个任务函数的参数，头尾分别用打印语句打印提示信息，并且在任务函数执行前后触发事件。

为了弄清楚执行顺序，我们注册好事件的监听函数，给定一个简单的任务函数，然后执行代码。

运行的结果如下：

    Before executing
    About to execute
    *** Executing task ***
    Done with execute
    After executing
    
注意，上述的结果说明代码执行是同步的，没有任何异步代码。

+ 首先输出`Before executing`
+ `begin`事件触发对应的监听函数，函数执行输出`About to execute`
+ 任务函数执行并且输出`*** Executing task ***`
+ `end`事件触发对应的监听函数，函数执行输出`Done with execute`
+ 最后输出`After executing`

正如回调一样，不要想当然地认为事件一定是同步或者异步的。

明白这点至关重要，如果给`execute`函数传入异步的`taskFunc`，事件触发时机就不准确了。

我们可以借助`setImmediate`函数模拟异步的函数：

    // ...
    
    withLog.execute(() => {
      setImmediate(() => {
        console.log('*** Executing task ***')
      });
    });

执行结果如下：

    Before executing
    About to execute
    Done with execute
    After executing
    *** Executing task ***
    
执行的结果是有问题的，异步调用之后的那些代码，即输出`Done with execute`，`After executing`的部分，不再是正确有效的提示。

若要在异步函数执行完毕之后触发事件，需要结合回调或者Promise对象。下文会具体讲到如何解决。

相对于一般的回调，事件触发的优点在于可以通过定义多个监听函数来达到一个事件触发多个函数的执行。如果用回调方式，需要在单个回调函数中写很多代码逻辑。

## 异步事件

我们将上面这个同步的例子再修改一下，变成实用一点的异步例子。
    
    const fs = require('fs');
    const EventEmitter = require('events');
    
    class WithTime extends EventEmitter {
      execute(asyncFunc, ...args) {
        this.emit('begin');
        console.time('execute');
        asyncFunc(...args, (err, data) => {
          if (err) {
            return this.emit('error', err);
          }
    
          this.emit('data', data);
          console.timeEnd('execute');
          this.emit('end');
        });
      }
    }
    
    const withTime = new WithTime();
    
    withTime.on('begin', () => console.log('About to execute'));
    withTime.on('end', () => console.log('Done with execute'));
    
    withTime.execute(fs.readFile, __filename);

`WithTime`类执行异步函数`asyncFunc`，通过`console.time`和` console.timeEnd`打印出异步函数执行所需的时间，并且在函数执行前后触发正确的事件。在异步函数的回调中，根据执行情况触发`error`或者`data`事件。

 我们传入异步函数`fs.readFile`来测试`WithTime`。 现在我们不再需要通过回调来处理读取后的文件数据，我们只要监听`data`事件就好了。
 
执行之后，我们得到正确的事件触发结果，也得到了函数执行所需的时间。

    About to execute
    execute: 4.507ms
    Done with execute
    
我们可以看到上述代码是如何结合回调和事件触发器完成的。如果`asyncFunc`支持Promise的话，我们还可以用async/await来代替。
    
    class WithTime extends EventEmitter {
      async execute(asyncFunc, ...args) {
        this.emit('begin');
        try {
          console.time('execute');
          const data = await asyncFunc(...args);
          this.emit('data', data);
          console.timeEnd('execute');
          this.emit('end');
        } catch(err) {
          this.emit('error', err);
        }
      }
    }

我不知道你怎么看，但对我来说这代码比起回调或者`.then`/`.catch`来说清晰多了。async/await特性让我们更近距离地接触JavaScript语言本身，我觉得非常棒。

## 事件参数和错误处理

上一个例子中，有两个事件触发时附带额外参数。

`error`事件触发时带有错误信息：

	this.emit('error', err);
	
而`data`事件对应的是数据信息：
    
    this.emit('data', data);

我们可以在事件参数后面带上任意多的参数，这些参数会作为对应监听函数的参数。

比如，我们传入的`data`参数会被注册的监听函数接收，而这个`data`对象正好是异步函数`asyncFunc`返回的结果数据。

    withTime.on('data', (data) => {
      // do something with data
    });

`error`事件比较特殊，在那个回调例子中，如果我们不人为处理错误事件，node进程会自动退出。

下面例子可以证明：

    class WithTime extends EventEmitter {
      execute(asyncFunc, ...args) {
        console.time('execute');
        asyncFunc(...args, (err, data) => {
          if (err) {
            return this.emit('error', err); // 未被处理
          }
    
          console.timeEnd('execute');
        });
      }
    }
    
    const withTime = new WithTime();
    
    withTime.execute(fs.readFile, ''); // 不好的调用
    withTime.execute(fs.readFile, __filename);
    
第一次调用会抛出错误，node进程崩溃然后自动退出；

    events.js:163
          throw er; // Unhandled 'error' event
          ^
    Error: ENOENT: no such file or directory, open ''

第二次调用受上一行的崩溃影响，根本就没有机会执行。

如果我们注册`error`事件的监听函数，结果就不一样。比如：

    withTime.on('error', (err) => {
      // 处理错误信息， 比如说打印出来
      console.log(err)
    });
    
如有上述代码存在，第一次调用的错误会被报告，node进程不会像之前一样崩溃退出。这也就意味着第二次调用正常进行：

    { Error: ENOENT: no such file or directory, open '' errno: -2, code: 'ENOENT', syscall: 'open', path: '' }
    execute: 4.276ms
    
但是，如果是Promise形式函数的话，Node中表现又会不一样，它只会输出警告：

    UnhandledPromiseRejectionWarning: Unhandled promise rejection (rejection id: 1): Error: ENOENT: no such file or directory, open ''
    DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.

处理`error`事件触发的异常的另一种方式是注册一个监听全局`uncaughtException`进程事件的函数，但这并不是个好主意。

一般情况下，建议避免使用`uncaughtException`。但如果非用不可（比如打印日志或者清理工作之类的），必须在监听函数中退出进程。

    process.on('uncaughtException', (err) => { 
      // 还不够
      console.error(err); 
    
      // 还需要强制推出进程
      process.exit(1);
    });
    
问题是，如果同时有多个错误事件触发，就会多次触发`uncaughtException`事件注册的监听函数，多次清理工作可能会造成问题。比如，当异常事件触发关闭数据库的动作时。

`EventEmitter`模块暴露一个`once`方法，限制了事件触发的监听函数只能被调用一次。它很适用未捕获异常的情况，因为只要第一次异常发生，我们就会开始清理，然后退出进程。

## 监听函数的顺序

如果给一个事件注册了多个监听函数，它们的调用是有序进行的。调用的顺序跟注册的顺序保持一致。

    // 第一个监听函数
    withTime.on('data', (data) => {
      console.log(`Length: ${data.length}`);
    });
    
    // 第二个监听函数
    withTime.on('data', (data) => {
      console.log(`Characters: ${data.toString().length}`);
    });

    withTime.execute(fs.readFile, __filename);

上述代码执行后，会先打印出Length这行信息，然后再打印Characters这行信息，因为这是监听函数的注册顺序。

如果想让定义在后面的监听函数先调用，可以通过`prependListener`方法：
    
    // 第一个监听函数
    withTime.on('data', (data) => {
      console.log(`Length: ${data.length}`);
    });
    
    // 第二个监听函数
    withTime.prependListener('data', (data) => {
      console.log(`Characters: ${data.toString().length}`);
    });

    withTime.execute(fs.readFile, __filename);
    
这样就会先打印出Characters这行信息了。

最后，如果想要移除某个监听函数，用`removeListener`方法。


