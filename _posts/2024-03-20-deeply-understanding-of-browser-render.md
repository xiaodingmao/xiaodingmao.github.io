---
layout: post
title: 深入理解页面如何渲染至浏览器
image: 
  path: /assets/img/blog/browser.png
description: >
  浏览器是一个多进程架构，每个标签页都是一个进程，每个进程都包括渲染进程、网络进程、GPU进程、插件进程、浏览器进程等。渲染进程负责将HTML、CSS、JavaScript等资源渲染成页面，是浏览器渲染的核心部分。
sitemap: false
---

# 深入理解页面如何渲染至浏览器

浏览器是多进程架构，每个进程又包含多个线程，进程之间通过PIC进行通信来协调工作。

主要分为以下4个进程：

- Browser process：位于top,负责协调其他进程，同时控制地址栏，书签，前进后退按钮
- render process:   控制标签页内的网站中展示的所有内容
- plugin process:  控制网站所有插件
- GPU process: 独立于其他进程，可以接受多个应用的请求，并在同一界面绘制

![browser](/assets/img/blog/browser.png)  

其中browser进程包含UI thread(控制浏览器按钮和输入框部分)，network thread(从网络上接收数据），storage process(控制文件的处理).

其中在用户输入网络地址后，UI线程会判定是否是有效的网络地址。如果是有效URL，此时tab会loading spinner,并将请求交给network thread,用于处理网络请求。network thread 会进行DNS解析，建立tcp连接，直到收到请求数据后，断开网络连接。

network thread 会读取相应数据，如果是zip文件或者其他格式文件，则将数据转为下载。若是html文件，则将获取到的数据交给渲染进程来渲染整个页面。
![browser](/assets/img/blog/renderProcess.png)  

渲染进程的主线程会处理大部分接收过来的数据。主线程进行html,css,javascript的解析。首先解析html数据构建DOM，在构建DOM时，遇到图片,CSS和javscript等外部资源，会并发运行预加载扫描器，并将请求发送到浏览器进程的网络线程进行数据请求。

当 HTML 解析器找到 `<script>` 标记时，它会暂停 HTML 文档的解析，并且必须加载、解析和执行 JavaScript 代码。因为 JavaScript 可以使用 `document.write()` 之类的内容更改整个 DOM 结构之类的内容来更改文档形状。可以向 `<script>` 标记添加 async 或 defer 属性。然后，浏览器会异步加载并运行 JavaScript 代码，而不会阻止解析。

主线程解析CSS并确定每个DOM节点的计算样式，然后在根据合成后的render树，构建布局树。

主线程会遍历 DOM 和计算出的样式，并创建布局树，其中包含 x y 坐标和边界框大小等信息。布局树的结构可能与 DOM 树类似，但它仅包含与页面上可见内容相关的信息。如果应用了 display: none，则该元素不属于布局树（然而，具有 visibility: hidden 的元素在布局树中）。同样，如果应用包含类似 p::before{content:"Hi!"} 的伪元素，它就会包含在布局树中，即使它不在 DOM 中也是如此。

之后主线程会遍历布局树来创建绘制记录，主要为了确定绘制顺序。

在浏览器已经了解了文档的结构、每个元素的样式、页面的几何图形以及绘制顺序后，同时在遍历布局树时候建立层树（z-index), 然后主线程会将该信息提交到合成器线程。然后，合成器线程会光栅化每个图层。图层的大小可能相当于页面的全部长度，因此合成器线程会将它们分成多块图块，并将每个图块发送到光栅线程。光栅线程会光栅化每个图块并将它们存储在 GPU 内存中。

图块光栅化后，合成器线程会收集图块信息（称为“绘制四边形”）来创建合成器框架。然后，通过 IPC 将合成器帧提交到浏览器进程。

合成的好处是，在不涉及主线程的情况下完成。合成器线程不需要等待样式计算或 JavaScript 执行。因此，仅合成动画被认为是实现流畅性能的最佳选择。如果需要再次计算布局或绘制，则必须涉及主线程。
## 以下是整体的渲染流程图
![browser](/assets/img/blog/renderWhole.png)  
