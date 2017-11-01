关于本文：

[原文地址](https://scotch.io/tutorials/build-a-progressive-web-app-offline-git-trending-app-part-1-concepts-and-service-workers)

[译文地址](https://github.com/Ge-yuan-jun/gittrends-pwa/blob/master/articles/%E6%9E%84%E5%BB%BA%E7%A6%BB%E7%BA%BFweb%E5%BA%94%E7%94%A81.md)

译者：墨白

------

我喜欢移动app，而且也是那些坚持使用Web技术构建移动应用程序的人之一。

经过技术的不断迭代（可能还有一些其它的东西），移动体验设计愈来愈平易近人，给予用户更好的体验。

而今天，我们就要介绍一个新技术--渐进式 web 应用程序。在理解这个概念并自己尝试了一下之后，我觉得没有必要再做 hybrid 应用了。

我们准备做这样的一个demo：

![demo预览](https://github.com/Ge-yuan-jun/gittrends-pwa/blob/master/articles/img/pwa-demo-1.png)

## Progressive Web Apps

渐进式 Web 应用是典型的旨在提高用户离线体验的 Web 应用。它解决了这样的问题：怎么才能不显示类似下面的离线错误？

![离线error](https://github.com/Ge-yuan-jun/gittrends-pwa/blob/master/articles/img/pwa-demo-2.jpg)

事实上，PWA 不仅解决了离线错误，还在恢复连接的时候将用户与内容连接起来。移动设备是渐进式 web 应用的主要使用场景。让我来告诉你为什么？

### 桌面浏览器
- 用户打开电脑（在家、学校或者办公室）
- 检查是否连上网络，没有则手动连接
- 打开 web 应用

### 移动端浏览器
- 拿出手机
- 默认手机已经连接上网络
- 直接打开 app

如上，用户对待两种场景的处理方式是不一样的。移动端用户不一定有很好的网络连接，有的甚至没有。在这样的场景下，开发商需要做的就是保持用户对产品的好感，在其网络恢复时与其互动。如果信号很差，开发商需要通过一些手段保持用户的耐心，不至于在请求过程中用户直接关闭 web 应用。

当我们开始构建 PWA 应用时，你就能理解上面的场景了。

## Service Workers
PWA 背后的原理是 service workers。如果想让用户在离线场景下依然保持打开 web 页面，你需要在用户打开 web 应用并且有网络连接时做一些“后台任务”，这个“后台任务”会搜集 web 页面最近一次运行需要的一些资源，以备离线时使用。

这就好像每年秋收储备粮食，以备冬天不时之需一样，不断循环。

PWA 中的 service worker，可以类比成春天的播种的农民。下面是 MDN 对 service workers 的描述：
> Service worker 是一个注册在指定源和路径下的事件驱动 worker。它采用 JavaScript 控制关联的页面或者网站，拦截并修改访问和资源请求，细粒度地缓存资源。你可以完全控制应用在特定情形（最常见的情形是网络不可用）下的表现。

简而言之，service worker 就是一些在后台运行逻辑的 worker。它没有权限操作 DOM，但是可以调用其它的 API （例如 IndexDB 以及 Fetch API）。

开始之前请牢记：
- service workers 只能在 HTTPS 协议下生效（或者 Localhost）。
- service workers 被设计成异步的，不能使用 XHR （但你可以使用 Fetch）或者 LocalStorage。
- service workers 的作用范围是针对相对路径的。因此，`demo/sw.js` 只能相对于 `demo` 起作用，`demo/first/sw.js` 相对于 `first`。

## Mobile 还是 PWA
如果你能利用 service workers 存储离线使用所需的文件，那你就没有必要开发移动 app 了。如果你的 web 应用对移动用户进行了优化，并且几乎不需要调用移动端的硬件功能，那么你应该尝试一下 PWA。

我花了一些时间看飞行模式下一些移动 app 的表现。我将它们分成三类：

### 离线情况下不做任何操作

例子： Coinbase

![Coinbase](https://github.com/Ge-yuan-jun/gittrends-pwa/blob/master/articles/img/pwa-coinbase.jpg)

Coinbase 就是一直停留在 loading 的这个页面。它甚至让我怀疑这样的 app 为啥要存在，因为这个页面简直跟 web 展示一模一样。Coinbase 不是财经类 app，无需实时展示信息，因此，PWA 可能只适用应用于其 App Shell。

> App Shell 是指不包含动态内容的一部分应用程序。例如导航菜单、侧边栏、背景、logo 等等。

### 离线情况下展示警告信息（未连接网络等等），展示 App Shell，但其它都不可用

例子：Uber

![Uber](https://github.com/Ge-yuan-jun/gittrends-pwa/blob/master/articles/img/pwa-uber.jpg)

Uber 给用户展示了一些信息（通过 App Shell 以及地图），并且告知用户不能操作是由于他网络中断了。Uber是一个很高频的 app，这样的交互展示对于他们的应用场景很有意义。

### 离线情况下展示缓存的数据
例子： Medium

![Medium](https://github.com/Ge-yuan-jun/gittrends-pwa/blob/master/articles/img/pwa-medium.jpg)

Medium在离线状态下展示缓存的数据，一些离线展示在这个分类里面的 app（例如，Instagram）还会提示用户离线了，所以，就不要对这个分类里面的 app 期望再搞了。

## 优化
我的想法是，如果 PWA（或者 service workers）技术成熟并且被大规模应用的话，为什么不节省掉：
1. 前往应用商店
2. 下载并不常用的 app
呢？

当我们接下来谈到 Web Manifest 时，你就意识到只要给你的 web 应用新增一个桌面 icon，web 应用就可以通过点击这个 icon 实现启动了。

一些公司已经在 PWA 方面做的比较好了，你可以在这个网址上面找到这些公司：[pwa.rocks](https://pwa.rocks/)

## 开发准备

我们已经介绍了足够多的理论知识了。这是一个手把手的教程，来吧，让我们动起手来。首先，按照下面的结构来创建一个新的项目：

```
|--pwa-demo
|----css
|----fonts
|----images
|----js
|----index.html
|----service-worker.js
```

下载 [Materialize](http://materializecss.com/) 这个 UI 库，用里面 `CSS`、`Fonts`、`js 文件`分别替换项目里面的文件夹。

打开 `index.html` 文件，引入一些资源：

```html
<!-- ./index.html -->
<!DOCTYPE html>
  <html>
    <head>
      <!--Import Google Icon Font-->
      <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
      <!--Import materialize.css-->
      <link type="text/css" rel="stylesheet" href="css/materialize.min.css" media="screen,projection"/>
      <link type="text/css" rel="stylesheet" href="css/app.css">

      <!--Let browser know website is optimized for mobile-->
      <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    </head>

    <body>

      Body coming soon

      <!-- Scripts -->
      <script type="text/javascript" src="js/jquery-2.1.1.min.js"></script>
      <script type="text/javascript" src="js/materialize.min.js"></script>
      <script type="text/javascript" src="js/app.js"></script>
    </body>
  </html>
``` 

我们已经引入了下载好的文件，还需要自己在相应的目录创建一下 `app.css` 以及 `app.js` 这两个文件。

## 注册 Service Worker

越早在浏览器注册，Service Worker 就能越早的开始工作。最佳的做法是在应用的入口。在这个项目中，我们可以在 `app.js` 注册一个新的 worker：

```js
(function(){
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker
     .register('/service-worker.js')
     .then(function() { 
        console.log('Service Worker Registered'); 
      });
  }    
})()
```

在做其他操作之前，我们首先需要检测一下浏览器对于 Service Worker 的兼容性。如果支持，那我们就可以利用 `register` 这个方法来注册这个 worker，这个方法告知了 service worker 文件的路径。注册函数返回一个 promise ，你可以在这个 promise 里面判断注册是否成功。

## Service Worker 周期

在开始构建 PWA 之前，你需要理解 Service Worker 的生命周期：

### Install

这一阶段主要是让 worker 在浏览器给定的作用域挂载。由于这是生命周期的第一步，最好在这一步缓存各种资源：

```js
// ./service-worker.js

var cacheName = 'PWADemo-v1';
var filesToCache = [
  '/index.html',
  '/css/app.css',
  '/js/app.js',
  /* ...and other assets (jQuery, Materialize, fonts, etc) */
];

self.addEventListener('install', function(e) {
  console.log('[ServiceWorker] Install');
  e.waitUntil(
    caches.open(cacheName).then(function(cache) {
      console.log('[ServiceWorker] Caching app shell');
      return cache.addAll(filesToCache);
    })
  );
});
```

- `caches.open` 和 `cache.addAll` 都是异步操作.service worker 在这些操作完成之前可能会中断,`e.waitUntil`用来等待 promise 的状态变成 resolved 或者 rejected。
- 当缓存开关被打开时，我们尝试利用 `addAll` 来新增缓存。
- 请记住，只要有一个文件缓存失败，service worker 就无法被正确挂载。

### Activate

当 worker 挂载完成，其效果并不会立即展示出来，除非前一个 service worker 销毁并且该 web 应用被重新访问。假设我们挂载了另一个不同 cacheName 的 service worker:

```
// ./service-worker.js

var cacheName = 'PWADemo-v2';
var filesToCache = [
  //...
];

self.addEventListener('install', function(e) {
  console.log('[ServiceWorker] Install');
  //...
});
```

当这个新的 service worker 创建之后，新的缓存 `PWADemo-v2` 也被创建，这时候 `PWADemo-v1` 仍然存在。当触发 Activate 时，我们可以删除 `PWADemo-v1`，使其“让位”于 `PWADemo-v2`：

```js
// ./service-worker.js

self.addEventListener('activate', function(e) {
  console.log('[ServiceWorker] Activate');
  e.waitUntil(
    caches.keys().then(function(keyList) {
      return Promise.all(keyList.map(function(key) {
        if (key !== cacheName) {
          console.log('[ServiceWorker] Removing old cache', key);
          return caches.delete(key);
        }
      }));
    })
  );
});
```

我们检查所有的 cache 名称，如果发现不是正在使用的 cache，那么将其直接删除。

### Fetch

Fetch 不是一个必需的生命周期，但它提供了拦截请求资源的方法。当发送请求时，首先会触发这样的事件：

```js
// ./service-worker.js

self.addEventListener('fetch', function(e) {
  console.log('[ServiceWorker] Fetch', e.request.url);
  e.respondWith(
    caches.match(e.request).then(function(response) {
      return response || fetch(e.request);
    })
  );
});
```

如果资源已经被缓存了，我们返回浏览器缓存的版本。如果没有，那么我们调用 fetch api 去发送 HTTP 请求该资源。

## Debuggering Service Workers

由于 service workers 的工作方式，特别是进行缓存时，不是很容易进行 debugger 调试。幸运的是，chrome 的 dev tools 提供了助力。跟着下面的步骤，调试我们刚注册的 service worker：

- 打开 chrome dev tools
- 点击 Application 这一选项，打开 service worker 分区：

![chrome dev tools](https://github.com/Ge-yuan-jun/gittrends-pwa/blob/master/articles/img/pwa-debugger-1.jpg)

- 你可以查看到 status 是绿色的，这就表明你的 service worker 成功了：

![status](https://github.com/Ge-yuan-jun/gittrends-pwa/blob/master/articles/img/pwa-debugger-2.jpg)

- 你可以打开 "Update on reload" 去强制更新 service worker，不用关闭所有已存在的 session：

![update on reload](https://github.com/Ge-yuan-jun/gittrends-pwa/blob/master/articles/img/pwa-debugger-3.jpg)

- 右击 "Cache Storage"，然后点击刷新去查看缓存。根据名称点击你所设置的cache，然后你就会看到缓存里面的各个项：

![Cache Storage](https://github.com/Ge-yuan-jun/gittrends-pwa/blob/master/articles/img/pwa-debugger-4.jpg)

## 接下来

你已经了解了必备的知识点，PWA 的概念对你来说已经不陌生了。接下来，我们将要讨论 PWA 的缓存策略。我们将了解如何使用 IndexDB 来保存数据而不是 localStorage。
