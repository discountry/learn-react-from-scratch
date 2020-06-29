# 《从零学习 React 技术栈·理论篇》教程简介

> Hello大家好，我是余博伦，在接下来的一段时间里，由我和大家从零开始共同学习React技术栈的相关知识。教程将会以连载的形式发布在我的个人博客和微信公众号上，以文字教程为主，辅以一些代码示例供同学们参考，在连载结束之后，我会将所有内容整理为电子书提供下载。连载时教程会在每日早晨由公众号推送，同时为了方便一些外链和代码示例，可以在本博客查看教程。
> 
> 作者：[余博伦](https://yubolun.com/)
> 
> 教程谢绝任何形式转载。

**《从零学习 React 技术栈·实战篇》** 已在 [Gitchat](https://gitbook.cn/gitchat/column/59ae12fdbc511269a95f9616) 发布！

React 是由 Facebook 主导开发的一个 JavaScript 框架。其实它和之前一些流行过的 MVVM 框架，例如 Angular 不同，React 主要只专注于 MVC 中的 V，也就是视图层。React 是当前最流行的，专门用来构建前端UI界面的框架。

React 的优点是它很快、很轻。并且它组件化的思想在开发构建界面时也对我们有很大的帮助，React 风格的代码，在我们学过之后就会了解到，它这种很规范化的写法，在一个需要共同开发协作的项目组或团队中，也是非常有益处的。

就好像100个人写 jQuery 就可能会有100种写法，但是不管让谁来写 React 组件，我们都能保证他写出来的代码和标准是差不多的。

所以假如你对 JavaScript 已经有了相当的掌握，想要学习框架来开发一些Web应用的话，选择 React 准是没错的。

现在的 React 已经不仅仅是一个框架，它逐步发展成了一个非常庞大的生态体系。

本教程会介绍React全家桶当中最为通用流行的一些框架和库，内容主要涉及到 React/React-router/Redux 这几个库，通过学习呢，在教程的结尾我们将能够开发完成一个利用上述三个工具库实现的 TodoList 应用。

现在你去看React官网文档，或者一些比较新的 React 教程，我们在书写 JavaScript 的时候呢，都已经开始采用 ES6 的语法，这些语法乍看起来可能会比较陌生。不过实际上，ES6实现的语法糖和一些新的方法 是能够极大地方便我们的编码的。

教程使用的是目前React发行的最新版本(v15.6.1，同时 v15.5.*  之后的版本的代码也是适用于React@16的)新版本当中引入了非常多的 ES6 的新特性，所以代码看起来和我们以前习惯的 jQuery 风格代码会有很多的不同。

在正式学习 React 技术栈之前，推荐你最好掌握一些 ES6 的基本语法，例如箭头函数、Class类、let、const等一些新的声明变量的方法等等，当然如果你不了解也别担心，在接下来的教程当中，每当涉及到这些新的语法时，我也会稍作简介。

**[JavaScript 参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference)**

## 教程主要内容介绍

教程主要会分为六个部分进行讲解，在简要的基础知识准备和开发环境搭建之后，我们会分别对React/Redux/react-router的关键知识点进行学习，之后我们还会介绍到如何在React应用中编写样式，在最后一个部分，我们将一同实现一个运用上述React技术栈实现的Todolist应用。

在第一部分我们将会一同学习本教程的主角，也就是如何使用React。在这一部分当中，我们不光会介绍如何编码，也会讲解React当中一些关键的原理，以及在开发过程中运用到的 的一些最佳实践等内容。

接下来我们会一同学习Redux的使用，首先我会帮助大家理解状态管理到底是一个什么样的概念，之后会依次介绍redux当中几个关键的概念如何实现，如何编写以及如何运用。

再然后呢，我们会介绍到前端路由的概念。react技术栈当中，有一个非常棒的库，react-router来实现前端路由的功能，比较特殊的是，这个库目前以及发行到了4.0版本，而之前的版本也都在维护，各个大版本之间有比较多的变化，一些比较细节的内容，即使是它的官方文档也没有写清楚。我在这里呢，会着重教给大家最新版的使用方法，以及一些官方文档都没有提到的配置方法和小技巧。

我们还会学习到如何在 React 应用中开发编写样式，在这里部分，我会介绍几种比较主流流行的解决方案供大家参考，你可以在日后正式的工作当中，自行选取你觉得最为合适的开发样式的方法。

在教程的最后一个部分，我们将会学习到开发Rect应用的一般思路，以及利用之前学过的知识一同实现一个TodoList应用。   

有的同学呢可能还会有一些疑问，比方说，现在还有一些别的很流行的框架，为什么我非得选 React 不可呢？

首先，React 相较于其他框架，其生态圈发展最为完整成熟，有非常多现成的完整的解决方案。并且它适用于大中型应用的开发，便于团队中多人之间协作，很多大厂就在正式的项目中使用了 React，并且学会 React 之后，你的能力将不止局限于浏览器之中，React还可以拓宽到开发原生应用，以及刚刚发布的ReactVR虚拟现实，甚至是物联网等各个领域。

## 为什么会有这个系列教程？

互联网上什么都有，杂乱无章。信息太多，相当于没有信息，选择太多，相当于没有选择。React 的中文资源比较少，大多数都已经过时，使用的是一两年前的版本，跟不上官方的版本更迭，且有一些中文资料由于翻译的不准确存在一些知识性的错误，很有可能会误导初学者。

中文的学习资源还是太少，而且良莠不齐。国内前端学习者普遍英文水平还不够，况且现在前端发展这么快，等学好英语考过四六级，说不定 React 已经过气了。

另外，网上还没有综合 React 技术栈所有库的最新版本的教程和代码示例。一些教程虽然非常优秀，但随着 React 及相关库的新版本发布，这些教程已经过期，甚至提供的示例代码已经不能正常运行了。

## 本教程相较其他React学习资源的优点在哪里？

我在准备教程的过程中查阅大量资料，综合了国内外所有优秀的 React 学习资源，萃取其中最精华的知识点，选择最为流行的 React 技术栈，立足最新版本的官网文档，在帮助新手入门上手的同时，也会对一些重要的知识概念进行讲解，满足初级、中级各个学习阶段和水平的同学。

全部采用当前发布的正式版本库进行教学，确保我用起来是这个样子，你学完之后用起来也是这个样子。

## 本教程的前置知识

想要学习本教程的同学最好对 JavaScript 基础知识、ES6 新特性等相关内容掌握了解。熟悉 JavaScript 中变量、对象、函数等基本概念，ES6 中const/class/箭头函数/解构赋值等新语法的基本使用。本教程 90% 的内容为 JavaScript 相关知识，学习者仅需熟悉基础的 HTML/CSS 即可。

## 本教程使用的框架版本及软件依赖

**框架及库**

*  react@15.6.1
*  redux@3.7.2
*  react-router@4.2.0

**软件及工具**

*  npm/cnpm
*  webpack
*  create-react-app
*  codepen/codepan
*  vs code
*  chrome
*  [VS Code React技术栈代码补全插件](https://marketplace.visualstudio.com/items?itemName=discountry.react-redux-react-router-snippets)

**代码示例**

*  [React on Codepen](https://codepen.io/collection/XbwydM)
*  [react-router on Codepen](https://codepen.io/collection/nJRQgQ)
*  [Redux on Codepen](https://codepen.io/collection/XWGqkp)
*  [React Demo Project on Github](https://github.com/discountry?utf8=%E2%9C%93&tab=repositories&q=react&type=&language=)

**学习资源**

*  [React官方中文文档](https://doc.react-china.org/)
*  [余博伦个人博客](https://yubolun.com/)
*  [知乎专栏·从零学习React技术栈](https://zhuanlan.zhihu.com/reactjs)
*  [知乎专栏·LeanReact](https://zhuanlan.zhihu.com/leanreact)
*  [知乎专栏·pure render](https://zhuanlan.zhihu.com/purerender)
