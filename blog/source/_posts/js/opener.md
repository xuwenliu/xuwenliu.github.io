---
title: JavaScript 浏览器新开窗口的正确方式
index_img: /img/js.jpg
tags: [JS]
category: 技术
abbrlink: f94fb1be
date: 2020-05-13 20:43:00
---
前端项目开发过程中，网页里的a标签默认在当前窗口跳转链接地址，如果需要在新窗口打开，需要给 a 标签添加一个`target="_blank"`属性。 例：
```js
<a target="_blank" href="https://weibo.com/">打开微博页面</a>
```

### 1.此操作有安全问题
如果只是加上 `target=“_blank'`，在打开新窗口之后，新页面可以通过 `window.opener` 来获取到来源页面的 `window` 对象，即使是跨域状态也一样。

![opener](/img/life/opener.png)

某些属性的访问被拦截，是因为跨域安全策略的限制。但是，如果我修改 `window.opener.location` 的值，指向另外一个地址。即可以造成刚刚还在某个网站浏览，随后点击链接新开的窗口，但是这个新开的窗口在神不知鬼不觉的情况下，把原来的网页地址改了，再通过页面伪装，例如伪装成登录页等，来让用户输入账号密码登录，从而达到黑客不可告人的目的。

### 2.此操作有性能问题
通过 `target="_blank"` 打开的新窗口，跟原来的页面窗口共用一个进程。如果这个新页面执行了一大堆性能不好的 JavaScript 代码，占用了大量系统资源，那你原来的页面的性能也会收到影响。

### 3.解决办法
1.尽量不使用  `target="_blank" `
2.如果一定要用，需要加上 `rel=”noopener“` 或者 `rel=“noreferrer”` ,这样新开窗口的  `window.openne`r 就为`null`，而且会让新窗口运行在独立的进程里，不会拖累原来页面的进程。(不过，有些浏览器对性能做了优化，即使不加这个属性，新窗口也会在独立进程打开。不过为了安全考虑，还是加上吧。)

下图是vscode的安全提示:
![tip](/img/life/vscode-tip.png)

3.通过 `window.open` 的方式打开新页面

```js
let newWindow = window.open();
newWindow.opener = null;
newWindow.location = "https://baidu.com/";
newWindow.target = "_blank";
```