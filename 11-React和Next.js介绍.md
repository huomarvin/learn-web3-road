在前面我们已经使用HTML、CSS和一些JavaScript代码编写了一个简单的dApp，现实工作中我们不会用这么老旧的方式去开发。

今天，我们使用`web framework`来简化网络开发过程。但它是否更简单？这要看你从谁的角度看。如果你刚刚起步，以前从未做过，可能需要一些时间，直到你理解所有必要的概念。但是，从长远来看，你会感谢自己，并为你花时间学习而感到高兴。

当前最流行的框架是

- [React](https://reactjs.org/)
- [Angular](https://angular.io/)
- [Vue](https://vuejs.org/)

虽然它们各有优缺点，但到目前为止，React已经在Web开发领域掀起了风暴。在Web3领域也是如此，React是构建dApps最常用的网络框架。在后面我们将使用React，所以本篇文章可以看成React的速成课程，它将教会你足够的东西来使用。这并不是要取代在专注于Web2教学的平台上学习React，而是要作为一个指南，了解有多少东西是必要的，以便你不会陷入教程的地狱。

> 实际上，我们将使用Next.js--它是React本身的一个扩展--但后面会有更多的内容。让我们先学习一些React。

## 什么是React

React是一个`web framework`，它使得构建和推理你的`web app`的 `view`变得容易。`view`是在屏幕上显示的内容，它如何变化，如何更新，等等。React本质上只是给你一个模板语言，你可以创建返回HTML的Javascript函数。

正常的Javascript函数会返回Javascript式的东西--字符串、数字、布尔值、对象等。React基本上结合了Javascript和HTML，产生了一种他们称之为JSX的语言。在JSX中，类似Javascript的函数返回HTML，而不是常规的Javascript事物。

返回HTML的Javascript函数的组合被称为`Components`。`Components`是用JSX编写的。虽然一开始看起来很笨拙，但一旦你习惯了，它们实际上是很容易操作的。

下面是一个简单组件的例子。

```jsx
function ShoppingList() {
  return (
    <div className="shopping-list">
      <h1>Shopping List</h1>
      <ul>
        <li>Apples</li>
        <li>Bananas</li>
        <li>Grapes</li>
      </ul>
    </div>
  );
}
```



