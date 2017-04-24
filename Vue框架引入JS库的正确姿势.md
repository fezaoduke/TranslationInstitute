
关于本文：

[原文地址](http://vuejsdevelopers.com/2017/04/22/vue-js-libraries-plugins/?jsdojo_id=reddit_lip&utm_content=bufferf1f60&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer)   [翻译地址](https://github.com/fezaoduke/TranslationInstitute/blob/master/Vue%E6%A1%86%E6%9E%B6%E5%BC%95%E5%85%A5JS%E5%BA%93%E7%9A%84%E6%AD%A3%E7%A1%AE%E5%A7%BF%E5%8A%BF.md)   译者：野草

在Vue.js应用中，可能需要引入Lodash，Moment，Axios，Async等非常好用的JavaScript库。当项目变得复杂庞大，通常会将代码进行模块化拆分。可能还需要跑在不同的环境下，比如浏览器，服务端。

如何在各个模块和组件文件中引入需要的库呢？ 找到一种简单靠谱的方式，可以省去很多的麻烦。

## 错误示范

### 全局变量法

最不靠谱的方式就是将导入的库挂在全部变量`window`对象下：

**entry.js**

    window._ = require('lodash');
    
**MyComponent.vue**

    export default {
      created() {
        console.log(_.isEmpty() ? 'Lodash everywhere!' : 'Uh oh..');
      }
    }

这种方式的缺点有很多，我们只说其中一个关键的点：不支持服务端渲染。当应用跑在服务端时，`window`对象不存在，当然试图去访问`window`下的属性会抛出错误。

### 处处引入法

另外一个不太优雅的方式就是在需要的每个文件中都引入对应的库：

**MyComponent.vue**

    import _ from 'lodash';
    
    export default {
      created() {
        console.log(_.isEmpty() ? 'Lodash is available here!' : 'Uh oh..');
      }
    }
    
    
虽然这方法是可行的，但是太不简洁。你必须在每个文件中都记得引入， 而且如果不需要了，又得重新删除。另外，如果打包策略不够明智的话，可能会打包出多份重复的代码。

## 正确引入方式  

最简单靠谱的方式是用库变成`Vue`的原型对象的属性。下面，我来演示如何将Moment库引入：

**entry.js**

    import moment from 'moment';
    Object.definePrototype(Vue.prototype, '$moment', { value: moment });
    
由于所有的组件都会继承`Vue`原型对象上的方法，因此这些方法在任何组件文件中都能通过`this.$moment`访问到：

**MyNewComponent.vue**

    export default {
      created() {
        console.log('The time is ' . this.$moment().format("HH:mm"));
      }
    }
    
我们来仔细看一下其中的原理。

### Object.defineProperty

通常我们会如下设置对象属性：

    Vue.prototype.$moment = moment;
    
你也可以这么做，但是`Object.defineProperty`允许我们用[属性描述器](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)来定义我们的属性。我们可以定义该属性是否可写，可枚举，可配置。

一般情况下，我们不需要用那么复杂的方式来赋值属性。但这里用它有个好处：用属性描述器定义的属性是默认只读的。

这能防止那些脑子不清醒的开发者（很可能是你哦~~）犯下一些低级错误：

    this.$http = 'Assign some random thing to the instance method';
    this.$http.get('/'); // TypeError: this.$http.get is not a function
    
`Object.defineProperty`能保护引入的库不被重新赋值，如果你尝试重写，程序会抛出“TypeError: Cannot assign to read only property”的错误。

### $

可能你注意到，我们用“$”开头的属性来存放引入的库。当然，你应该记得还有其他的一些属性也是这样的，比如`$refs`， `$on`， `$mount`。

这种做法不是强制的，这个前缀就是为了提醒某些昏昏沉沉的开发者（怎么又是你？！），这些属性是公有的，你可以在任何地方使用。反之，某些属性只能在Vue内部使用。

作为一门以原型为基本的语言，JavaScript中并没有真正的类，所以也就没有所谓的公有，私有变量，或者静态方法。上面这种约定，我觉得是种不错的选择。

### this

现在你能用`this.$libraryName`的方式来访问你需要的库了。但，你得保证`this`的指向。如果你在回调函数中使用`this`，通常这个`this`不再指向Vue实例。

箭头函数是解决这个问题的好方法。

    this.$http.get('/').then(res => {
      if (res.status !== 200) {
        this.$http.get('/') // etc
        // Only works in a fat arrow callback.
      }
    });
    
## 写成插件

如果你在项目的很多地方都用了某个库，或者你希望全局可用，你可以构建自己的Vue插件。

插件能化繁为简，能让你像下面这样很简单地引入自己想要的库：

    import MyLibraryPlugin from 'my-library-plugin';
    Vue.use(MyLibraryPlugin);

就像Vue Route，Vuex等插件一样，我们的库仅仅需要两行，就能在任何地方使用了。

### 如何写插件

首先，创建一个文件。本例中，我将引入一个Axios库的插件。我们就把这个文件命名为`axios.js`吧。

最关键的地方在于，我们需要暴露一个将Vue构造器作为第一个参数的`install`方法。

**axios.js**

    export default {
      install: function(Vue) {
        // Do stuff
      }
    }
    
然后我们可以用之前的方式将库添加到Vue的原型对象上：

**axios.js**

    import axios from 'axios';
    
    export default {
      install: function(Vue) {
        Object.defineProperty(Vue.prototype, '$http', { value: axios });
      }
    }
    
接着我们只需要Vue实例的`use`方法就能将这个库引入整个项目了。我们像下面代码一样简单引入：

**entry.js**

    import AxiosPlugin from './axios.js';
    Vue.use(AxiosPlugin);
    
    new Vue({
      created() {
        console.log(this.$http ? 'Axios works!' : 'Uh oh..');
      }
    })
    
## 插件参数设置

插件的`install`方法还可以接受其他的可选参数。有些开发者可能不喜欢Axios实例对象的方法名`$http`，因为Vue resource插件的方法名也是这个。然后，让我们利用第二个参数来修改它。

**axios.js**

    import axios from 'axios';
    
    export default {
      install: function(Vue, name = '$http') {
        Object.defineProperty(Vue.prototype, name, { value: axios });
      }
    }

**entry.js**

    import AxiosPlugin from './axios.js';
    Vue.use(AxiosPlugin, '$axios');
    
    new Vue({
      created() {
        console.log(this.$axios ? 'Axios works!' : 'Uh oh..');
      }
    })
    
当然上面，我们可以直接在`Object.defineProperty`的中将`name`属性写死成`$axios`。也可以在`install`方法中引入多个需要的库。

【译者话】这篇文章浅显易懂，实用小巧。早上看到的，马上就应用在了项目中。对我来说，是涨知识了。你呢？
