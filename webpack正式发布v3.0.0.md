
> 原文链接：[ webpack 3: Official Release!!](https://medium.com/webpack/webpack-3-official-release-15fd2dd8f07b) 
> 
> 作者：[Sean T. Larkin](https://medium.com/@TheLarkInn)
>
> 翻译：野草
>
> 本文首发于前端早读课[【第975期】](https://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651226628&idx=2&sn=3cccf4c34a2df4970682aa7f86753846&chksm=bd495b808a3ed296198efabd2a4f6eccba27abdfc87a6e70b24c4827d368195ba218126b3c08&mpshare=1&scene=1&srcid=07029B9pOQmqQ7uIZovyOAdR&key=50814c5812fb30a2f0c04b633bbd82d2b1403ea5063ac4db9c084b08dbae5fb7942881435732f85584564b0909c376682431273a5bec65b20624219c7981dc81a9a7581780a80d28b46605451306b7c9&ascene=0&uin=MTQ2MjQ2MjU4MQ%3D%3D&devicetype=iMac+MacBookPro13%2C2+OSX+OSX+10.12.2+build)

发布webpack 2之后，我们就跟社区承诺：我们将发布大家投票出来的功能特性，而且会保证更快更稳定的发布周期。

不会再有一年才出来的测试版本，不会再有不兼容的重大改变。我们承诺我们会保证大家的权益，因为你们才使得webpack如此欣欣向荣。

现在webpack团队很自豪地跟大家宣布，今天（2017.06.20）正式发布了webpack 3.0.0！现在就可以下载升级了！！！

	npm install webpack@3.0.0 --save-dev
或者

	yarn add webpack@3.0.0 --dev

从webpack 2迁移到webpack 3只需要上述简简单单的一条命令行就完成升级。由于webpack内部的一些会影响到插件的大更改，我们把这个版本定为全新的版本号。

## 新特性

如上所述，我们更新的是你们[投票](https://webpack.js.org/vote)出来的特性。正因为Github上庞大的代码贡献，赞助者以及拥护者的支持，我们才得以一一击破这些新特性。

### 作用域提升（Scope Hoisting）

作用域提升是wepack 3的核心特性。webpack在打包时会把每个模块用单独的闭包封装起来，这些闭包会减速代码的执行过程，但这是webpack在性能和？？之间作出的一个权衡。相比之下，诸如Closure Compiler和RollupJS之类的打包工具“提升”或者说打通了所有模块的作用域，将文件打包到一个闭包函数中，提高了代码的执行速度。
 
 现在有了webpack 3，可以在配置上中添加如下插件来实现作用域提升的功能：

	module.exports = {  
	  plugins: [
	    new webpack.optimize.ModuleConcatenationPlugin()
	  ]
	};

作用域提升是ECMAScript标准中模块的新语法。webpack打包也可能会回退到常规的打包策略，这取决于你采用的是哪种模块语法（ES模块语法还是其他模块语法），当然还有[其他条件](https://medium.com/webpack/webpack-freelancing-log-book-week-5-7-4764be3266f5)。

想知道回退的原因，可以添加`--display-optimization-bailout`属性，它会在回退的时候通知你并显示原因。

由于作用域提升移除了模块的闭包函数，打包后的代码可能会变小。不过，最关键的提升点是代码执行速度。如果你有很好的对比例子，可以告诉我们，我们很乐意与大家分享。


###  魔法注释（Magic Comments）

当我们告诉大家webpack 2可以使用动态引入语法（`import()`）的时候，开发者反馈说它不能像`require.ensure`一样命名打包之后文件的名字。

现在我们引入了社区所谓的魔法注释特性，它能让开发者在`import()`语句中以内联注释的方式传入打包后文件的名字，[以及其他更多配置](https://medium.com/webpack/how-to-use-webpacks-new-magic-comment-feature-with-react-universal-component-ssr-a38fd3e296a)。

	import(/* webpackChunkName: "my-chunk-name" */ 'module');
这些特性在webpack 2.4 和 2.6 版本中已经发布，但是v3版本修复了已有的bug，保证了特性的稳定性。现在，我们可以像使用`require.ensure`一样灵活地使用动态引入语法了。

想了解更多，点击查看我们[最新的“代码分割”官方文档](https://webpack.js.org/guides/code-splitting-async)。

## 下一步

我们希望带给你们更多的新特性和原有代码改善。点击[投票页面](https://webpack.js.org/vote/)，选出你最期待的特性，我们将根据大家的意愿来优先完善。

以下是我们正在做的：

+ 更好的构建缓存
+ 更快的初次和渐进构建体验
+ 更好的TypeScript体验
+ 长期缓存改进
+ WASM模块支持
+ 用户体验提升
 
## 附录

[Git仓库](https://github.com/webpack/webpack/releases)上列出列 webpack 2.6.1到wepback 3.0.0的新特性：
 
+ node_modules no longer mangle to ~ in stats(重大改变)
+ HMR请求可配置超时时间
+ 新增试验性特性作用域提升
+ 性能提升
+ 新增`output.libraryExport`配置导出的库
+ `sourceMapFilename`支持`[contenthash]`(重大改变)
+ `module.noParse`支持函数
+ 新增`node: false`配置项使所有node配置失效
