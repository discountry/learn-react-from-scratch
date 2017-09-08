这节课我们来介绍 Redux 当中的 Reducer.

在之前的课程中，我们定义了这样一个函数：

```js
const counter = (state = { value: 0 }, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { value: state.value + 1 };
    case 'DECREMENT':
      return { value: state.value - 1 };
    default:
      return state;
  }
}
```

counter函数内部呢是一个switch结构，它接受两个参数，state和action，并返回一个新的state，我们可以把这样的函数抽象为：

```
(previousState, action) => newState
```

在Redux中，我们把像这样的，根据应用现有状态和触发的action返回新的状态的函数称为reducer.

话又说回来，为什么这样的函数就被称之为reducer呢？

我们注意到redux的官方文档里专门有一句对reducer命名的解释：

> It's called a reducer because it's the type of function you would pass to [Array.prototype.reduce(reducer, ?initialValue)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

应该翻译为：

> 之所以将这样的函数称之为reducer，是因为这种函数与被传入 `Array.prototype.reduce(reducer, ?initialValue)` 的回调函数属于相同的类型。

为什么这么讲呢？我们来看一个array使用reduce方法的具体例子：

```js
// 这里的callback是和reducer非常相似的函数
// arr.reduce(callback, [initialValue])

var sum = [0, 1, 2, 3].reduce(function(acc, val) {
  return acc + val;
}, 0);
// sum = 6

/* 注意这当中的回调函数 (prev, curr) => prev + curr
 * 与我们redux当中的reducer模型 (previousState, action) => newState 看起来是不是非常相似呢
 */
[0, 1, 2, 3, 4].reduce( (prev, curr) => prev + curr );
```

我们再来看一个简单的具体的reducer的例子：

```js
// reducer接受state和action并返回新的state
const todos = (state = [], action) => {
  // 根据不同的action.type对state进行不同的操作，一般都是用switch语句来实现，当然你要用if...else我也拦不住你
  switch (action.type) {
    case 'ADD_TODO':
      return [
        // 这里使用的是展开运算符语法
        ...state,
        {
          id: action.id,
          text: action.text,
          completed: false
        }
      ];
    // 不知道是什么action类型的话则返回默认state
    default:
      return state;
  }
};
```

如果非要翻译reducer的话，可以将其翻译为缩减器或者折叠器？

为了进一步加深理解，我们再了解一下reduce是什么东西，这个名词其实是函数式编程当中的一个术语，在更多的情况下，reduce操作被称为Fold折叠。

直观起见，我们还是拿JavaScript来理解。reduce属于一个高阶函数，它将其中的回调函数reducer递归应用到数组的所有元素上并返回一个独立的值。这也就是“缩减”或“折叠”的意义所在了。

**总而言之一句话，redux当中的reducer之所以叫做reducer，是因为它和 `Array.prototype.reduce` 当中传入的回调函数非常相似。**

当然，如果你认为这种命名不完美容易产生歧义，你完全可以去给redux提交一个PR，提供一种更加恰当的命名方式。

```js
const count = function(state, action) {
    if(action.type == 'INCREMENT') {
        return state + 1;
    } else if(action.type == 'DECREMENT') {
        return state - 1;
    } else {
        return state;
    }
}
```

Reducer 的主体是一个switch结构的运算。要注意记得我们刚才提到的reducer的模型，它只是根据传入的状态数据state和action来判断返回一个新的state. reducer必须是一个纯函数，纯函数主要的含义就是它不可以修改影响输入值，并且没有副作用，副作用指的是例如函数中一些异步调用或者会影响函数作用域之外的变量一类的操作。

另外，注意我们返回新的state时使用的展开操作符的方法，在上面的示例中，这样返回的是一个全新的数组，而不是修改了传入的数组。这也就是我们使用 React 技术栈时尤其需要注意的一点：保证数据的immutability不可变性。

如果state的数据是一个普通的数字或者字符串我们直接也返回数字或字符串就可以了，但是为什么数组或者对象会需要这些特别的返回方式呢？我们可以来做一下实验：

```js
var a = 1
var b = a
b = 2
console.log(a)
console.log(b)

var oldArray = [1, 2, 3]
var newArray = oldArray
newArray[1] = 233
console.log(oldArray)
console.log(newArray)

var prevArray = [4, 5, 6]
var nextArray = prevArray.slice()
// var nextArray = [...prevArray]
nextArray[1] = 666
console.log(prevArray)
console.log(nextArray)
```

我们先声明两个整数类型的变量a和b，先给a赋值等于1，之后赋值b等于a，然后我们将b赋值为2，之后查看两个变量的值，a还是1，而b变成了2.

那么如果我们的变量类型是数组的话，会发生什么情况呢？还是声明两个变量，这次是两个数组类型的变量，我们把oldArray赋值为包含元素1，2，3的数组，之后声明newArray，并赋值它等于oldArray，之后再将newArray的第二个元素修改为233，然后我们再来观察一下，我们会发现oldArray和newArray中第二个元素都被修改成了233，也就是说我们在修改newArray时oldArray也受到了影响。这是因为在复制时，我们直接写 newArray = oldArray只是对数组进行了浅拷贝，只是相当于给oldArray多起了一个别名，它俩指向的还是同一个数组。

这时我们就需要对我们的变量进行一些特别的操作。我们再来试一下，这次还是声明两个数组类型的变量，首先声明prevArray是以4，5，6为元素的数组，之后我们为nextArray赋值，不同的是，这里我们使用数组原生的slice方法不传入参数。这时我们再修改next Array中第二个元素的值，我们再来观察一下，这一次两个数组直接没有相互影响，我们通过slice方法实现了对数组变量的深拷贝，像之前的例子中，我们使用ES6的展开操作符也能取得相同的效果。

那么有同学看到这里肯定就想问了，为什么我们要保证reducer返回的是一个新的state数据，而不能直接修改传入的数值呢？首先一点呢，在我们的React应用当中，数据发生改变，界面也要跟着响应改变重新渲染，而这一步之间是需要有一个比较差异的diff操作的，因此改变之前和改变之后的数据都需要使用到。如果我们使用和修改的只是一个数据，那么比较差异就无从谈起了，另外通过使用redux可以实现的一些诸如时间旅行的功能也需要依赖这些不可变的数据来实现。

