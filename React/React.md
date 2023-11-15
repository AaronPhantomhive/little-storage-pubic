# React学习总结

## React

“有个贴切的比喻，把DOM和JavaScript各自想象为一个岛屿，它们之间用收费桥梁连接，js每次访问DOM，都要途径这座桥，并交纳“过桥费”,访问DOM的次数越多，费用也就越高。 因此，推荐的做法是尽量减少过桥的次数，努力待在ECMAScript岛上。因为这个原因react的虚拟dom就显得难能可贵了，它创造了虚拟dom并且将它们储存起来，每当状态发生变化的时候就会创造新的虚拟节点和以前的进行对比，让变化的部分进行渲染。整个过程没有对dom进行获取和操作，只有一个渲染的过程，所以react说是一个ui框架。”


### React搭建项目

vite 搭建 react18 项目

`yarn create vite app-client --template react-ts`

https://zhuanlan.zhihu.com/p/518339176

https://vitejs.dev/

### React核心特点

- **声明式设计** −React采用声明范式，可以轻松描述应用。
- **组件** − 通过 React 构建组件，使得代码更加容易得到复用。功能独立，向外提供接口，可组装／组合。
- **DOM**：React通过对DOM的模拟，最大限度地减少与DOM的交互。



### React基础

#### 1、React.createElement()

`const title = React.createElement('h1', null, 'Hello React')`

参数1：元素名称
参数2：元素属性
参数3（及其后面的元素）：元素的子节点

#### 2、React组件

[组件](https://zh-hans.reactjs.org/docs/components-and-props.html#function-and-class-components)分为函数组件以及类组件。

函数组件：

```react
function Hello() {
  return (
    <div>first 组件</div>
  )
}
// 渲染组件
ReactDOM.render(<Hello/>, document.getElementById('root'))
```

类组件：

```react
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>; 
  }
}

// 渲染组件:
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

函数组件以及类组件对比：

| 函数组件                                                | 类组件                                                       |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| 是使用JS中的函数创建的组件                              | 用 ES6 的 class 来定义组件                                   |
| 函数组件必须有return值                                  | 类组件必须提供render()方法<br />render()方法必须有return值，表示该组件的结构 |
| 使用函数名作为组件签名                                  | 类组件应该继承React.Component父类，<br />从而可以使用父类中提供的方法或属性 |
| 函数组件为无状态（state）组件<br />负责数据展示（静态） | 类组件为有状态（state）组件<br />负责更新UI（动态）          |

#### 3、React元素

- 元素都是不可变的。当元素被创建之后，是无法改变其内容或属性的。

- 目前更新界面的唯一办法是创建一个新的元素，然后将它传入 ReactDOM.render() 方法： 

```react
// 官方计时器例子
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>现在是 {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
  	// 参数一：要渲染的react元素
    element,   
    // 参数二：挂载点。dom对象，用于指定渲染到页面中的位置
    document.getElementById('example')
  );
}

setInterval(tick, 1000);
```

附：[挂载官方文档](https://zh-hans.reactjs.org/docs/react-component.html#mounting)

以上实例通过 setInterval() 方法，每秒钟调用一次 ReactDOM.render()。
我们可以将要展示的部分封装起来，以下实例用一个函数来表示：

```react
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>现在是 {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    // 封装 clock
    <Clock date={new Date()} />,
    document.getElementById('example')
  );
}

// 之后调tick
setInterval(tick, 1000);
```

#### 4、React封装例子

```react
function HelloMessage(props) {
    return <h1>Hello World!</h1>;
}

const element = <HelloMessage />;

ReactDOM.render(
    element,
    document.getElementById('example')
);
```

#### 5、React事件处理

React 元素的事件处理和 DOM 元素类似。但是有一些语法上的不同:

- React 事件绑定属性的命名采用驼峰式写法，而不是小写。

- 如果采用 JSX 的语法需要传入一个函数作为事件处理函数，而不是一个字符串(DOM 元素的写法)

语法： `onClick = {{} => {}}`

HTML 通常写法是：

```react
<button onclick="activateLasers()">
  激活按钮
</button>
```

React 中写法为：

```react
<button onClick={activateLasers}>
  激活按钮
</button>
```

在 React 中另一个不同是你不能使用返回 false 的方式阻止默认行为， 你必须明确使用 `preventDefault`。

例如，通常我们在 HTML 中阻止链接默认打开一个新页面，可以这样写：

```react
<a href="#" onclick="console.log('点击链接'); return false">
  点我
</a>
```

在 React 的写法为：

```react
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('链接被点击');
  }

  return (
    <a href="#" onClick={handleClick}>
      点我
    </a>
  );
}
```

#### 6、React判断条件

在 JSX 中不能使用 if else 语句，但可以使用 conditional (三元运算) 表达式来替代。[详情见此处](https://blog.csdn.net/shi851051279/article/details/79873386)。

#### 7、React箭头函数

箭头函数表达式，引入箭头函数有两个方面的作用：更简短的函数，并且不绑定 this。

```react
var f = ([参数]) => 表达式（单一）
// 等价于以下写法
var f = function([参数]){
   return 表达式;
}
```

箭头函数的基本语法如下：

`(参数1, 参数2, …, 参数N) => { 函数声明 }`

`(参数1, 参数2, …, 参数N) => 表达式（单一）`

相当于：

`(参数1, 参数2, …, 参数N) =>{ return 表达式; }`

当只有一个参数时，圆括号是可选的：

`(单一参数) => {函数声明}`

`单一参数 => {函数声明}`

没有参数的函数应该写成一对圆括号：

`() => {函数声明}`

#### 8、Function.prototype.bind()

利用ES5中的bind方法，将事件处理程序中的this与组件实例绑定在一起。

```react
constructor() {
  super()
  // 这个onIncrement
  this.onIncrement = this.onIncrement.bind(this)
}
render() {
  return(
    //...
    // 和这个onIncrement
    <button onClick = {this.onIncrement}> + 1</button>
    //...
  )
  //...
}
```

#### 9、setState()修改状态

语法：`this.setState({要修改的数据})`

```react
// 正确写法
this.setState({
  count: this.state.count + 1
})
// 错误写法，不要直接修改state中的值
this.state.count + 1
```

#### 10、React表单：**受控组件 vs 非受控组件**

- [受控组件](https://zh-hans.reactjs.org/docs/forms.html#controlled-components) 

  - 概念：
    - 在 HTML 中，表单元素（如`<input>`、 `<textarea>` 和 `<select>`）通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 setState()来更新。
    - 把两者结合起来，使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。（这句话可以理解成：React将state与表单元素value绑定在一起，由state的值来控制表单元素的值）
    - 被 React 以这种方式控制取值的表单输入元素就叫做 **“受控组件”**。
    - **总结为：其值受到react控制的表单元素。**
  - 受控组件创建步骤：
    - 在state中添加一个状态，作为表单元素的value值（控制表单元素值的来源）
    - 给表单元素绑定change事件，将表单元素的值设置为state的值（控制表单元素值的变化）
  - [示例教学视频](https://www.bilibili.com/video/BV14y4y1g7M4?p=36)

- 非受控组件

  - 概念：

    - 在一个受控组件中，表单数据是由 React 组件来管理的。另一种替代方案是使用非受控组件，这时表单数据将交由 DOM 节点来处理。
    - 非受控组件借助于ref，使用原生DOM方式来获取表单元素值
    - ref的作用：获取DOM或组件

  - 非受控组件创建步骤：

    - 调用React.createRef()方法创建ref对象

      ```react
      constructor() {
      	super()
      	this.txtRef = React.createRef()
      }
      ```

    - 将创建好的ref对象添加到文本框中

      ```react
      <input type="text" ref={this.txtRef} />
      ```

    - 通过ref对象获取到文本框的值

      ```react
      Console.log(this.txtRef.current.value)    
      ```




### React进阶

#### 1、props基本使用

- 组件是封闭的，想要接收外部数据，应该用props来实现。

- props的作用：接受传递给组件的数据。

- 传递数据：给组件标签添加属性。

- 接收数据：函数组件通过参数props接受数据，类组件通过this.props接受数据。

props.name使用方法：

```react
<Hello name = "jack" age = (19)>

function Hello(props) {
  console.log(props)
  return (
    <div>接收数据：{props.name}</div>
  )
}
```

this.props.name使用方法：

```react
class Welcome extends React.Component {
  render() {
    return {
      <div>接收数据：{this.props.name}</div>;
    }
  }
}

<Hello name = "jack" age = (19)>
```

#### 2、props特点

- 可以给组件传递任意类型的数据

- props是只读对象，只能读取值，无法修改对象

- 注意：使用类组件时，如果写了构造函数，应该将props传递给super(),否则无法在构造函数中获取到props

```react
class Welcome extends React.Component {
  constructor(props) {
    super(props)
  }
  render() {
    return(
      <div>接收数据：{this.props.name}</div>;
    )
  }
}
```

#### 3、Context使用步骤

调用React.createContext() 创建Provider（提供数据）和 Consumer（消费数据）两个组件

```react
const(Provider, Consumer) = React.createContext() 
```

使用Provider组件作为父节点

```react
<Provider>
	<div className="App">
		<child1>
	</div>
</Provider>
```

设置value属性，表示要传递的数据

```react
<Provider value="aaaa">
```

调用Consumer组件接收数据

```react
<Consumer>
    {data => <span>data参数表示接受到的数据 -- {data}</span>}
</Consumer>
```

#### 4、组件的[生命周期](https://www.runoob.com/react/react-component-life-cycle.html)

可分成三个状态：

- Mounting(挂载)：已插入真实 DOM

- Updating(更新)：正在被重新渲染

- Unmounting(卸载)：已移出真实 DOM

#### 5、高阶组件 [HOC](https://zh-hans.reactjs.org/docs/higher-order-components.html)

高阶组件（Higher-Order Components）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。

具体而言，**高阶组件是参数为组件，返回值为新组件的函数。**

```react
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```



### React钩子函数

#### 0、钩子（Hook）概念

可以理解成，组件尽量写成纯函数组件。如果需要外部功能和副作用，就使用Hook将外部代码“钩”过来。

react规定钩子一律使用useXXXX命名。

一个简单的有状态组件：

```react
class Example extends React.Compoent {
	constructor(props) {
        super(props);
        this.state = {
            count: 0
        };
    }

	render() {
    	return (
    		<div>
            	<p> You clicked {this.state.count} times</p>
            	<button onclick={() => this.setState({ count: this.state.count + 1})}>
            		Click me
            	</button>
        	</div>
    	);
    }
}
```

一个最简单的Hooks：

```react
import { useState } from 'react';

function Example() {
    const [count, setCount] = useState(0);
    
    return (
    	<div>
            <p> You clicked {count} times</p>
            <button onclick={() => setCount(count + 1)}>
            	Click me
            </button>
        </div>
    );
}
```

Example变成了一个函数。

但这个函数却有自己的状态（count），同时它还可以更新自己的状态（setCount）。

useState这个hook让函数变成了一个有状态的函数。

**注意：Hook不可以在以下情况下使用：**

- 循环（loop）
- 条件（if）
- 嵌套函数
- 普通函数
- callback（useEffect属于callback）

**可以使用的地方：**

- 函数组件的最顶层
- React 函数中（可以在 Hook 中调用其他的 Hook）

#### 1、[useState](https://zh-hans.reactjs.org/docs/hooks-state.html)

纯函数组件没有状态，这个钩子用于为函数组件引入状态(state)。

useState是react自带的一个hook函数，意思是声明一个状态变量，并给它一个初始值0。useState会返回一个数组，包含两个元素：当前的状态值和一个更新状态的函数。可以用数组解构的语法来给它们取名，比如：

```react
const [count, setCount] = useState(0);
```

这样就可以在组件中使用count变量来读取状态值，或者使用setCount函数来更新状态值，并触发组件重新渲染。


如果不用数组解构的话，可以写成下面这样。实际上数组解构是一件开销很大的事情，用下面这种写法，或者改用对象解构，性能会有很大的提升：

```react
let _useState = useState(0);
let count = _useState[0];
let setCount = _useState[1];
```

读取状态值：

```react
<p> You clicked {count} times</p>
```

状态count此时就是一个单纯的变量，不再需要写成 `{this.state.count}`。

更新状态：

```react
<button onclick={() => setCount(count + 1)}>
    Click me
</button>
```

注意点：

- useState接收的初始值没有规定一定要是string/number/boolean这种简单数据类型，它完全可以接收对象或者数组作为参数。

- 唯一需要注意的点是，之前我们的 this.setState做的是合并状态后返回一个新状态，而 useState是直接替换老状态后返回新状态。

#### 2、[useEffect](https://zh-hans.reactjs.org/docs/hooks-effect.html)

副作用处理钩子。副作用（side effects）是指那些在组件内部无法处理的操作，比如网络请求、手动修改DOM、日志记录等。

ps: useEffect还可以接受一个数组作为第二个参数，这个数组表示副作用的依赖项。如果依赖项没有变化，React就不会重新执行副作用。

useEffect使用示例：

```react
import { useState, useEffect } from 'react';

function Example() {
    const [count, setCount] = useState(0);
    
    useEffect(() => {
        // 更新文档的标题
        document.title = 'You clicked ${count} times';
    }, [count]); // ← [count]接受数组作为第二个参数表示副作用的依赖项

    return (
    	<div>
            <p> You clicked {count} times</p>
            <button onclick={() => setCount(count + 1)}>
            	Click me
            </button>
        </div>
    );
}
```

以上是使用useEffect的情况。

如果完全不使用Hook，则写成：

```react
class Example extends React.Compoent {
	constructor(props) {
        super(props);
        this.state = {
            count: 0
        };
    }
    
    componentDidMount() {
        document.title = 'You clicked ${this.state.count} times';
    }
    
    componentDidUpdate() {
        document.title = 'You clicked ${this.state.count} times';
    }
    
    render(){
    	return (
    		<div>
            	<p> You clicked {count} times</p>
            	<button onclick={() => this.setState({ count: this.state.count + 1})}>
            		Click me
            	</button>
	        </div>
    	);   
    }
}
```

注意：

- react首次渲染和之后的每次渲染都会调用一遍传给useEffect的函数。同：之前要用两个声明周期函数来分别表示首次渲染（componentDidMount），和之后的更新导致的重新渲染（componentDidUpdate）。
- useEffect中定义的副作用函数的执行不会阻碍浏览器更新视图，这些函数是异步执行的，而之前的componentDidMount或componentDidUpdate中的代码则是同步执行的。



ps: react18之后，开发模式下，StrictMode时，useEffect会加载两次，是为了模拟立即卸载组件和重新挂载组件。

The right question isn’t “how to run an Effect once,” but “how to fix my Effect so that it works after remounting”. 正确的解决办法不是“怎么样让 Effect 执行一次”，而是“怎样修复我的 Effect，让它在(重复)挂载之后正常工作”

具体解决办法详见：https://blog.csdn.net/qq_34164814/article/details/127750672




#### 3、[useReducer](https://zh-hans.reactjs.org/docs/hooks-reference.html#usereducer)

useReducer 则是 hooks 提供的一个类似于 redux 的 api，让我们可以通过 action 的方式来管理 context，或者 state。

```react
//reducer.js
const initialState = {count: 0};

function reducer(state, action) {
    switch (action.type) {
        case 'reset':
            return initialState;
        case 'increment':
            return {count: state.count + 1};
        case 'decrement':
            return {count: state.count - 1};
    }
}

export default function Counter({initialCount}) {
    const [state, dispatch] = useReducer(
        //  const [state, dispatch] = useReducer(reducer, initialState);
        reducer, initialState,
        {type: 'reset', payload: initialCount},  // 则在初始渲染期间应用初始操作
    );
    return (
        <>
            Count: {state.count}
            <button onClick={() => dispatch({type: 'reset'})}>
                Reset
            </button>
            <button onClick={() => dispatch({type: 'increment'})}>+</button>
            <button onClick={() => dispatch({type: 'decrement'})}>-</button>
        </>
    );
}
```

#### 4、[useContext](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecontext)

共享状态钩子

接受一个 context（上下文）对象（从React.createContext返回的值）并返回当前 context 值，由最近 context 提供程序给 context 。

`const context = useContext(Context);`

当提供程序更新时，此 Hook 将使用最新的 context 值触发重新渲染。

#### 5、[useCallback](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecallback)

```react
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

返回一个 [memoized](https://en.wikipedia.org/wiki/Memoization) 回调函数。

`useCallback` 的真正目的在于缓存了每次渲染时 inline callback 的实例。

`React.memo` 和 `React.useCallback` （或者 `PureComponent`）一定需要配对使用，缺了一个都可能导致性能不升反“降”。

#### 6、**[useMemo](https://zh-hans.reactjs.org/docs/hooks-reference.html#usememo)**

```react
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

返回一个 [memoized](https://en.wikipedia.org/wiki/Memoization) 值。

#### 7、[额外Hook API](https://zh-hans.reactjs.org/docs/hooks-reference.html)

- [基础 Hook](https://zh-hans.reactjs.org/docs/hooks-reference.html#basic-hooks)
  - [`useState`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usestate)
  - [`useEffect`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useeffect)
  - [`useContext`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecontext)
- [额外的 Hook](https://zh-hans.reactjs.org/docs/hooks-reference.html#additional-hooks)
  - [`useReducer`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usereducer)
  - [`useCallback`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecallback)
  - [`useMemo`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usememo)
  - [`useRef`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useref)
  - [`useImperativeHandle`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useimperativehandle)
  - [`useLayoutEffect`](https://zh-hans.reactjs.org/docs/hooks-reference.html#uselayouteffect)
  - [`useDebugValue`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usedebugvalue)
- [教程详解](https://www.jianshu.com/p/2e177ad95594)

#### 8、自定义Hook例子

`TypeUseSelectorHook Interface`：

```react
export interface TypeUseSelectorHook<TState> {
    // 这里传的值是一个泛型，可以在之后进行定义数据类型
	<TSelected>(
		selector: (state: TState) => TSelected,
		equalityFn?: (left: TSelected, right: TSelected) => boolean
	): TSelected;
}
```

`create Hook`：

```react
export const useMBSelector: TypeUseSelectorHook<MBState> = createSelectorHook(MBStoreContext);
```

`use useMBSelector`：

```react
// 这里最后的nodeId是前面function()传过来的值，此时为useMBSelector return回来一个map（键值对），然后使用nodeId取里面的值（是个array）。
const nodeDAtaArray = useMBSelector(useOtherSelector(aId, bId), mapEquals(arrayEquals()))[nodeId];
// 同理，之后取一个value传回来给plotDataKeyIndex
const plotDataKeyIndex = useMBSelector(useTheOtherSelector(aId, bId, (def) => def.plotDataTitle)).value;
// 最后取值
const temp = Number(nodeDAtaArray[plotDataKeyIndex]);
let value = temp;
```



## Redux

"只有遇到 React 实在解决不了的问题，你才需要 Redux 。"

如果UI层非常简单，没有很多互动，Redux 就是不必要的，用了反而增加复杂性。

- 用户的使用方式非常简单
- 用户之间没有协作
- 不需要与服务器大量交互，也没有使用 WebSocket
- 视图层（View）只从单一来源获取数据

如果UI层多交互、多数据源。

- 用户的使用方式复杂
- 不同身份的用户有不同的使用方式（比如普通用户和管理员）
- 多个用户之间可以协作
- 与服务器大量交互，或者使用了WebSocket
- View要从多个来源获取数据

或从组件角度看，如果应用有以下场景，可以考虑使用 Redux。

- 某个组件的状态，需要共享
- 某个状态需要在任何地方都可以拿到
- 一个组件需要改变全局状态
- 一个组件需要改变另一个组件的状态



### Redux流程图

![ReduxFlow](ReduxFlow.gif)

### Redux基本概念以及API

- **state**：驱动应用的真实数据源头
- **view**：基于当前状态的 UI 声明性描述
- **[Action](http://cn.redux.js.org/tutorials/essentials/part-1-overview-concepts#action)**：纯声明式的数据结构，只提供事件的所有要素，不提供逻辑。
  - 根据用户输入在应用程序中发生的事件，并触发状态更新
  - **action** 是一个具有 `type` 字段的普通 JavaScript 对象。**你可以将 action 视为描述应用程序中发生了什么的事件**.
  - `type` 字段是一个字符串，给这个 action 一个描述性的名字，比如`"todos/todoAdded"`。我们通常把那个类型的字符串写成“域/事件名称”，其中第一部分是这个 action 所属的特征或类别，第二部分是发生的具体事情。
  - action 对象可以有其他字段，其中包含有关发生的事情的附加信息。按照惯例，我们将该信息放在名为 `payload` 的字段中。
- [**Reducer**](http://cn.redux.js.org/tutorials/essentials/part-1-overview-concepts#reducer)：reducer是一个匹配函数，action的发送是全局的：所有的reducer都可以捕捉到并匹配与自己相关与否，相关就拿走action中的要素进行逻辑处理，修改store中的状态，不相关就不对state做处理原样返回。
  - reducer接收当前的 `state` 和一个 `action` 对象，必要时决定如何更新状态，并返回新状态。
  - 函数签名是：`(state, action) => newState`。 
  - 可以将 reducer 视为一个事件监听器，它根据接收到的 action（事件）类型处理事件。
- **[Store](http://cn.redux.js.org/tutorials/essentials/part-1-overview-concepts#store)**：store 是通过传入一个 reducer 来创建的，并且有一个名为 `getState` 的方法。
- **[Dispatch](http://cn.redux.js.org/tutorials/essentials/part-1-overview-concepts#dispatch)**：Redux store 有一个方法叫 `dispatch`。**更新 state 的唯一方法是调用 `store.dispatch()` 并传入一个 action 对象**。 
  - store 将执行所有 reducer 函数并计算出更新后的 state，调用 `getState()` 可以获取新 state。
  - **dispatch 一个 action 可以形象的理解为 "触发一个事件"**。发生了一些事情，我们希望 store 知道这件事。 Reducer 就像事件监听器一样，当它们收到关注的 action 后，它就会更新 state 作为响应。
- [**Selector**](http://cn.redux.js.org/tutorials/essentials/part-1-overview-concepts#selector)：**Selector** 函数可以从 store 状态树中提取指定的片段。随着应用变得越来越大，会遇到应用程序的不同部分需要读取相同的数据。

**※※※Redux 期望所有状态更新都是使用不可变的方式**。

#### 1、Store

Store：保存数据的地方，可以把它看成一个容器。

整个应用只能有一个 Store。

Redux 提供`createStore`这个函数，用来**生成 Store**。

```react
import { createStore } from 'redux';
const store = createStore(fn);
```

上面代码中，`createStore` 函数接受另一个函数作为参数，返回新生成的 Store 对象。

#### 2、State

`Store`对象包含所有数据。如果想得到某个时点的数据，就要对 Store 生成快照。这种时点的数据集合，就叫做 State。

当前时刻的 State，可以通过`store.getState()`拿到。

```react
import { createStore } from 'redux';
const store = createStore(fn);

const state = store.getState();
```

Redux 规定， 一个 State 对应一个 View。只要 State 相同，View 就相同。知道 State，就知道 View 是什么样，反之亦然。

#### 3、Action

State 的变化，会导致 View 的变化。但是，用户接触不到 State，只能接触到 View。所以，State 的变化必须是 View 导致的。Action 就是 View 发出的通知，表示 State 应该要发生变化了。

Action 是一个对象。其中的`type`属性是必须的，表示 Action 的名称。其他属性可以自由设置，[规范](https://github.com/acdlite/flux-standard-action)参考。

Action 必须

- 是一个普通的 JavaScript 对象。
- 有一个`type`属性。

Action 可能

- 有一个`error`属性。
- 有一个`payload`属性。
- 有一个`meta`属性。

Action 不能包含`type`，`payload`，`error`，和`meta`以外的属性。

```react
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};
```

上面代码中，Action 的名称是`ADD_TODO`，它携带的信息是字符串`Learn Redux`。

可以这样理解，Action 描述当前发生的事情。改变 State 的唯一办法，就是使用 Action。它会运送数据到 Store。

`payload`

可选的`payload`属性可以是任何类型的值。它代表动作的有效载荷。任何不是Action的`type`状态或动作的有关Action的信息都应该是该`payload`字段的一部分。

按照惯例，如果`error`是`true`，则`payload`应该是错误对象。这类似于拒绝带有错误对象的承诺。

`error`

如果操作表示错误`error`，则可以将可选属性设置为`true`。

一个`error`为真的Action 类似于一个被拒绝的 Promise。按照惯例，`payload`应该是一个错误对象。

如果`error`有任何其他的价值之外`true`，包括`undefined`和`null`，Action 一定不能被解释为错误。

`meta`

可选`meta`属性可以是任何类型的值。它适用于不属于有效载荷的任何额外信息。

#### 4、Action Creator

View 要发送多少种消息，就会有多少种 Action。可以定义一个函数来生成 Action，这个函数就叫 `Action Creator`。

```react
const ADD_TODO = '添加 TODO';

function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}

const action = addTodo('Learn Redux');
```

上面代码中，`addTodo`函数就是一个 `Action Creator`。

#### 5、selector

类似于计算属性，由state派发出的新得数据

#### 6、store.dispatch()

`store.dispatch()`是 **View 发出 Action 的唯一方法**。

```react
import { createStore } from 'redux';
const store = createStore(fn);

store.dispatch({
  type: 'ADD_TODO',
  payload: 'Learn Redux'
});
```

上面代码中，`store.dispatch`接受一个 Action 对象作为参数，将它发送出去。

结合 Action Creator，这段代码可以改写如下：

```react
store.dispatch(addTodo('Learn Redux'));
```

#### 7、store.subscribe()

Store 允许使用`store.subscribe`方法设置`listener`，一旦 State 发生变化，就自动执行这个函数。

```react
import { createStore } from 'redux';
const store = createStore(reducer);

store.subscribe(listener);
```

把 View 的更新函数（对于 React 项目，就是组件的`render`方法或`setState`方法）放入`listener`，就会实现 View 的自动渲染。

`store.subscribe`方法返回一个函数，调用这个函数就可以解除监听。

```react
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);

unsubscribe();
```

#### 8、Store 的实现总结

Store 提供了三个方法:

- store.getState()
- store.dispatch()
- store.subscribe()

```react
import { createStore } from 'redux';
let { subscribe, dispatch, getState } = createStore(reducer);
```

`createStore`方法还可以接受第二个参数，表示 State 的最初状态

```react
let store = createStore(todoApp, window.STATE_FROM_SERVER)
```

`window.STATE_FROM_SERVER`就是整个应用的状态初始值。(注意，如果提供了这个参数，它会覆盖 Reducer 函数的默认初始值)

 Store 是怎么生成的:

```react
const createStore = (reducer) => {
  let state;
  let listeners = [];

  const getState = () => state;

  const dispatch = (action) => {
    state = reducer(state, action);
    listeners.forEach(listener => listener());
  };

  const subscribe = (listener) => {
    listeners.push(listener);
    return () => {
      listeners = listeners.filter(l => l !== listener);
    }
  };

  dispatch({});

  return { getState, dispatch, subscribe };
};
```



### Reducer

reducer是一个匹配函数，action的发送是全局的：所有的reducer都可以捕捉到并匹配与自己相关与否，相关就拿走action中的要素进行逻辑处理，修改store中的状态，不相关就不对state做处理原样返回。

#### 1、概念

Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。

Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。

```react
const reducer = function (state, action) {
  // ...
  return new_state;
};
```

整个应用的初始状态，可以作为 State 的默认值。下面是一个实际的例子。

```react
const defaultState = 0;
const reducer = (state = defaultState, action) => {
  switch (action.type) {
    case 'ADD':
      return state + action.payload;
    default: 
      return state;
  }
};

const state = reducer(1, {
  type: 'ADD',
  payload: 2
});
```

上面代码中，`reducer`函数收到名为`ADD`的 Action 以后，就返回一个新的 State，作为加法的计算结果。其他运算的逻辑（比如减法），也可以根据 Action 的不同来实现。

实际应用中，Reducer 函数不用像上面这样手动调用，`store.dispatch`方法会触发 Reducer 的自动执行。为此，Store 需要知道 Reducer 函数，做法就是在生成 Store 的时候，将 Reducer 传入`createStore`方法。

```react
import { createStore } from 'redux';
const store = createStore(reducer);
```

上面代码中，`createStore`接受 Reducer 作为参数，生成一个新的 Store。以后每当`store.dispatch`发送过来一个新的 Action，就会自动调用 Reducer，得到新的 State。

Reducer可以作为数组的`reduce`方法的参数。下面的例子，为一系列 Action 对象按照顺序作为一个数组：

```react
const actions = [
  { type: 'ADD', payload: 0 },
  { type: 'ADD', payload: 1 },
  { type: 'ADD', payload: 2 }
];

const total = actions.reduce(reducer, 0); // 3
```

数组`actions`表示依次有三个 Action，分别是加`0`、加`1`和加`2`。数组的`reduce`方法接受 Reducer 函数作为参数，就可以直接得到最终的状态`3`。

#### 2、Reducer 函数

Reducer 是纯函数，同样的State，可以保证得到同样的 View。

因此Reducer 函数里面不能改变 State，必须返回一个全新的对象：

```react
// State 是一个对象
function reducer(state, action) {
  return Object.assign({}, state, { thingToChange });
  // 或者
  return { ...state, ...newState };
}

// State 是一个数组
function reducer(state, action) {
  return [...state, newItem];
}
```

要得到新的 State，唯一办法就是生成一个新对象。这样的好处是，任何时候，与某个 View 对应的 State 总是一个不变的对象。

#### 3、Reducer 的拆分

Redux 提供了一个`combineReducers`方法，用于 Reducer 的拆分。只要定义各个子 Reducer 函数，然后用这个方法，将它们合成一个大的 Reducer。

```react
import { combineReducers } from 'redux';

const chatReducer = combineReducers({
  chatLog,
  statusMessage,
  userName
})

export default todoApp;
```

上面的代码通过`combineReducers`方法将三个子 Reducer 合并成一个大的函数。

这种写法有一个前提，就是 State 的属性名必须与子 Reducer 同名。如果不同名，就要采用下面的写法。

```react
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,
  c: c
})

// 等同于
function reducer(state = {}, action) {
  return {
    a: doSomethingWithA(state.a, action),
    b: processB(state.b, action),
    c: c(state.c, action)
  }
}
```

总之，`combineReducers()`做的就是产生一个整体的 Reducer 函数。该函数根据 State 的 key 去执行相应的子 Reducer，并将返回结果合并成一个大的 State 对象。

下面是`combineReducer`的简单实现。

```react
const combineReducers = reducers => {
  return (state = {}, action) => {
    return Object.keys(reducers).reduce(
      (nextState, key) => {
        nextState[key] = reducers[key](state[key], action);
        return nextState;
      },
      {} 
    );
  };
};
```

可以把所有子 Reducer 放在一个文件里面，然后统一引入。

```react
import { combineReducers } from 'redux'
import * as reducers from './reducers'

const reducer = combineReducers(reducers)
```



### Reducer中间件（middleware）

#### 1、中间件的添加

`store.dispatch()`方法，可以添加中间件。

举例来说，要添加日志功能，把 Action 和 State 打印出来，可以对`store.dispatch`进行如下改造。

```react
let next = store.dispatch;
store.dispatch = function dispatchAndLog(action) {
  console.log('dispatching', action);
  next(action);
  console.log('next state', store.getState());
}
```

中间件就是一个函数，对`store.dispatch`方法进行了改造，在发出 Action 和执行 Reducer 这两步之间，添加了其他功能。

#### 2、中间件的用法

[Redux 中间件都编写为一系列 3 个嵌套函数](https://redux.js.org/tutorials/fundamentals/part-4-store#writing-custom-middleware)：

- 外部函数接收一个“store API”对象with `{dispatch, getState}`
- 中间函数接收`next`链中的中间件（或实际`store.dispatch`方法）
- 内部函数将在`action`通过中间件链时被调用

中间件的实现：

```react
const thunkMiddleware = ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState)
    }
    return next(action)
}
```

中间件的次序规定：

```react
const store = createStore(
  reducer,
  applyMiddleware(thunk, promise, logger)
);
```

`applyMiddleware`方法的三个参数，就是三个中间件。

**有的中间件有次序要求，使用前要查一下文档**。比如，`logger`就一定要放在最后，否则输出结果会不正确。

#### 3、applyMiddlewares()

作用是将所有中间件组成一个数组，依次执行：

```react
export default function applyMiddleware(...middlewares) {
  return (createStore) => (reducer, preloadedState, enhancer) => {
    var store = createStore(reducer, preloadedState, enhancer);
    var dispatch = store.dispatch;
    var chain = [];

    var middlewareAPI = {
      getState: store.getState,
      dispatch: (action) => dispatch(action)
    };
    chain = middlewares.map(middleware => middleware(middlewareAPI));
    dispatch = compose(...chain)(store.dispatch);

    return {...store, dispatch}
  }
}
```

所有中间件被放进了一个数组`chain`，然后嵌套执行，最后执行`store.dispatch`。可以看到，中间件内部（`middlewareAPI`）可以拿到`getState`和`dispatch`这两个方法。

#### 4、[redux-promise](https://github.com/redux-utilities/redux-promise/blob/master/src/index.js) 中间件

让 Action Creator 返回一个 Promise 对象时使用`redux-promise`中间件：

```react
import { createStore, applyMiddleware } from 'redux';
import promiseMiddleware from 'redux-promise';
import reducer from './reducers';

const store = createStore(
  reducer,
  applyMiddleware(promiseMiddleware)
); 
```

这个中间件使得`store.dispatch`方法可以接受 Promise 对象作为参数。这时，Action Creator 有两种写法。

写法一，返回值是一个 Promise 对象。

```react
const fetchPosts = 
  (dispatch, postTitle) => new Promise(function (resolve, reject) {
     dispatch(requestPosts(postTitle));
     return fetch(`/some/API/${postTitle}.json`)
       .then(response => {
         type: 'FETCH_POSTS',
         payload: response.json()
       });
});
```

写法二，Action 对象的`payload`属性是一个 Promise 对象。这需要从[`redux-actions`](https://github.com/acdlite/redux-actions)模块引入`createAction`方法，并且写法也要变成下面这样。

```react
import { createAction } from 'redux-actions';

class AsyncApp extends Component {
  componentDidMount() {
    const { dispatch, selectedPost } = this.props
    // 发出同步 Action
    dispatch(requestPosts(selectedPost));
    // 发出异步 Action
    dispatch(createAction(
      'FETCH_POSTS', 
      fetch(`/some/API/${postTitle}.json`)
        .then(response => response.json())
    ));
  }
```

第二个`dispatch`方法发出的是异步 Action，只有等到操作结束，这个 Action 才会实际发出。

注意: `createAction`的第二个参数必须是一个 Promise 对象。

如果 Action 本身是一个 Promise，它 resolve 以后的值应该是一个 Action 对象，会被`dispatch`方法送出（`action.then(dispatch)`），但 reject 以后不会有任何动作；如果 Action 对象的`payload`属性是一个 Promise 对象，那么无论 resolve 和 reject，`dispatch`方法都会发出 Action。

#### 5、[redux-thunk](https://redux.js.org/usage/writing-logic-thunks#redux-thunk-middleware) 中间件

异步组件：

加载成功后（`componentDidMount`方法），它送出了（`dispatch`方法）一个 Action，向服务器要求数据 `fetchPosts(selectedSubreddit)`。这里的`fetchPosts`就是 Action Creator。

```react
class AsyncApp extends Component {
  componentDidMount() {
    const { dispatch, selectedPost } = this.props
    dispatch(fetchPosts(selectedPost))
  }
  // ...
}
```

此时`fetchPosts`返回了一个函数，而普通的 Action Creator 默认返回一个对象。

返回的函数的参数是`dispatch`和`getState`这两个 Redux 方法，普通的 Action Creator 的参数是 Action 的内容。

Action 是由`store.dispatch`方法发送的。而`store.dispatch`方法正常情况下，参数只能是对象，不能是函数。

这时，就要使用中间件[`redux-thunk(github链接)`](https://github.com/gaearon/redux-thunk)。

```react
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import reducer from './reducers';

// Note: this API requires redux@>=3.1.0
const store = createStore(
  reducer,
  applyMiddleware(thunk)
);
```

上面代码使用`redux-thunk`中间件，改造`store.dispatch`，使得后者可以接受函数作为参数。

因此，异步操作的第一种解决方案就是，写出一个返回函数的 Action Creator，然后使用`redux-thunk`中间件改造`store.dispatch`。

#### 6、[getDefaultMiddleware](https://redux-starter-kit.js.org/api/getDefaultMiddleware)

thunk 中间件确实有一个自定义选项。可以在设置时创建 thunk 中间件的自定义实例，并将“额外参数”注入中间件。然后中间件将这个额外的值作为每个 thunk 函数的第三个参数注入。这最常用于将 API 服务层注入 thunk 函数，以便它们对 API 方法没有硬编码的依赖。

`getDefaultMiddleware`返回一个包含默认中间件列表的数组。

**[自定义Included 中间件](https://redux-toolkit.js.org/api/getDefaultMiddleware#customizing-the-included-middleware)**：

`getDefaultMiddleware` 接受允许以两种方式自定义每个中间件的选项对象：

- 每个中间件都可以通过传递`false`其对应的字段来排除结果数组
- 每个中间件都可以通过为其相应字段传递匹配的选项对象来自定义其选项

Thunk setup with an extra argument:

```react
import thunkMiddleware from 'redux-thunk'

const serviceApi = createServiceApi('/some/url')

const thunkMiddlewareWithArg = thunkMiddleware.withExtraArgument({ serviceApi })
```

Redux Toolkit的`configureStore`用来支持它在`getDefaultMiddleware`中作为自定义中间件的一部分：

```react
import { configureStore } from '@reduxjs/toolkit'
import rootReducer from './reducer'
import { myCustomApiService } from './api'

const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      thunk: {
        extraArgument: { serviceApi } // { serviceApi }可以是个function可以是个参数，extraArgument: myCustomApiService,
      },
      serializableCheck: false,
    }),
})
```

只能有一个额外的参数值。如果您需要传入多个值，请传入包含这些值的对象。

然后 thunk 函数将接收该额外值作为其第三个参数：

```react
export const fetchTodoById = todoId => async (dispatch, getState, extraArgument) => {
    // In this example, the extra arg is an object with an API service inside
    const { serviceApi } = extraArgument
    const response = await serviceApi.getTodo(todoId)
    dispatch(todosLoaded(response.todos))
}
```

#### 7、thunk详解

[What the heck is a 'thunk'?](https://daveceddia.com/what-is-a-thunk/)

[thunk基础](https://medium.com/fullstack-academy/thunks-in-redux-the-basics-85e538a3fe60)

thunk就是可以利用 redux 中间件让 redux 支持异步的 action。

```react
const thunk = ({ dispatch, getState }) => (next) => (action) => {
  if (typeof action === 'function') {
    return action(dispatch, getState);
  }
  return next(action);
};
export default thunk
```

上为thunk实现代码，可以理解为：

如果 action 是个函数，就调用这个函数。

如果 action 不是函数，就传给下一个中间件。

再简化一点就是：发现 action 是函数就调用它。



### 异步操作

#### 1、发出 Action

同步操作只要发出一种 Action 即可，异步操作的差别是它要发出三种 Action：

- 操作发起时的 Action
- 操作成功时的 Action
- 操作失败时的 Action

思路：

- 操作开始时，送出一个 Action，触发 State 更新为"正在操作"状态，View 重新渲染
- 操作结束后，再送出一个 Action，触发 State 更新为"操作结束"状态，View 再一次重新渲染

以向服务器取出数据为例，三种 Action 可以有两种不同的写法。

```react
// 写法一：名称相同，参数不同
{ type: 'FETCH_POSTS' }
{ type: 'FETCH_POSTS', status: 'error', error: 'Oops' }
{ type: 'FETCH_POSTS', status: 'success', response: { ... } }

// 写法二：名称不同
{ type: 'FETCH_POSTS_REQUEST' }
{ type: 'FETCH_POSTS_FAILURE', error: 'Oops' }
{ type: 'FETCH_POSTS_SUCCESS', response: { ... } }
```

除了 Action 种类不同，异步操作的 State 也要进行改造，反映不同的操作状态：

```react
let state = {
  // ... 
  isFetching: true,
  didInvalidate: true,
  lastUpdated: 'xxxxxxx'
};
```

State 的属性`isFetching`表示是否在抓取数据。`didInvalidate`表示数据是否过时，`lastUpdated`表示上一次更新时间。

#### 2、new Promise()

Promise的构造函数接收一个参数，为函数，并且传入两个参数：resolve，reject，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数。

```react
function runAsync(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('执行完成');
            resolve('一些数据');
        }, 2000);
    });
    return p;            
}
runAsync()
```

`resolve()` 有点像 `callback()` 的用法。

reject()一般用来reject(e)。

```react
waitStore(selector: (state: MyState)) => boolean, signal? : AbortSignal): Promise<void>
```

#### 3、异步逻辑，承诺链和async/await

async request with promise chaining

承诺链无需阻塞等待返回结果。

```react
function fetchData(someValue) {
  return (dispatch, getState) => {
    dispatch(requestStarted())
    myAjaxLib.post('/someEndpoint', { data: someValue }).then(
        response => dispatch(requestSucceeded(response.data)),
        error => dispatch(requestFailed(error.message))
    )
  }
}
```

error handling with async/await

async/await阻塞请求，待请求结束后继续执行。

```react
function fetchData(someValue) {
  return async (dispatch, getState) => {
    dispatch(requestStarted())

    // Have to declare the response variable outside the try block
    let response

    try {
      response = await myAjaxLib.post('/someEndpoint', { data: someValue })
    } catch (error) {
      // Ensure we only catch network errors
      dispatch(requestFailed(error.message))
      // Bail out early on failure
      return
    }

    // We now have the result and there's no error. Dispatch "fulfilled".
    dispatch(requestSucceeded(response.data))
  }
}
```

#### 4、Redux异步数据流程图

![ReduxAsyncDataFlowDiagram](ReduxAsyncDataFlowDiagram.gif)



## React - Redux

React-Redux 将所有组件分成两大类：UI 组件（presentational component）和容器组件（container component）。

### UI 组件 & 容器组件

#### UI 组件特征

- 只负责 UI 的呈现，不带有任何业务逻辑
- 没有状态（即不使用`this.state`这个变量）
- 所有数据都由参数（`this.props`）提供
- 不使用任何 Redux 的 API

#### 容器组件特征

容器组件的特征恰恰相反。

- 负责管理数据和业务逻辑，不负责 UI 的呈现
- 带有内部状态
- 使用 Redux 的 API

如果一个组件既有 UI 又有业务逻辑，将它拆分成下面的结构：外面是一个容器组件，里面包了一个UI 组件。前者负责与外部的通信，将数据传给后者，由后者渲染出视图。



### React - Redux组件API

#### 1、connect()

React-Redux 提供 `connect` 的方法，用于从 UI 组件生成容器组件。**作用为连接React组件与 Redux store**。

```react
import { connect } from ‘react-redux’
const VisibleTodoList = connect()(TodoList);
```

`TodoList`是 UI 组件，`VisibleTodoList`就是由 React-Redux 通过`connect`方法自动生成的容器组件。

为了定义业务逻辑，需要给出下面两方面的信息：

- 输入逻辑：外部的数据（即`state`对象）如何转换为 UI 组件的参数
- 输出逻辑：用户发出的动作如何变为 Action 对象，从 UI 组件传出去。

因此，`connect`方法的完整 API 如下：

```react
import { connect } from 'react-redux'

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)
```

`connect`方法接受两个参数：`mapStateToProps`和`mapDispatchToProps`。

它们定义了 UI 组件的业务逻辑。前者负责输入逻辑，即将`state`映射到 UI 组件的参数（`props`），后者负责输出逻辑，即将用户对 UI 组件的操作映射成 Action。

#### 1.1、mapStateToProps()

**connect 的第一个参数是 mapStateToProps()。**

构建好Redux系统的时候，它会被自动初始化，但是你的React组件并不知道它的存在，因此你需要分拣出你需要的Redux状态，所以你需要绑定一个函数，它的参数是state，简单返回你关心的几个值。

它建立一个从（外部的）`state`对象到（UI 组件的）`props`对象的映射关系。

作为函数，`mapStateToProps`执行后应该返回一个对象，里面的每一个键值对就是一个映射。

这个函数允许我们将 store 中的数据作为 props 绑定到组件上。

```react
const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}
```

`state`作为参数，返回一个对象。这个对象有一个`todos`属性，代表 UI 组件的同名参数。

后面的`getVisibleTodos`也是一个函数，可以从`state`算出 `todos` 的值。

`getVisibleTodos`的一个例子，用来算出`todos`：

```react
const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
    default:
      throw new Error('Unknown filter: ' + filter)
  }
}
```

#### 1.2、mapDispatchToProps()

connect **的第二个参数是** mapDispatchToProps。

**（声明好的action作为回调，也可以被注入到组件里，就是通过这个函数，它的参数是dispatch，通过redux的辅助方法bindActionCreator绑定所有action以及参数的dispatch，就可以作为属性在组件里面作为函数简单使用了，不需要手动dispatch。这个mapDispatchToProps是可选的，如果不传这个参数redux会简单把dispatch作为属性注入给组件，可以手动当做store.dispatch使用。）**

由于更改数据必须要触发`action`, 因此在这里的主要功能是将 `action` 作为`props` 绑定到 组件上。

它用来建立 UI 组件的参数到`store.dispatch`方法的映射。

也就是说，它定义了哪些用户的操作应该当作 Action，传给 Store。它可以是一个函数，也可以是一个对象。

如果`mapDispatchToProps`是一个函数，会得到`dispatch`和`ownProps`（容器组件的`props`对象）两个参数。

```react
const mapDispatchToProps = (
  dispatch,
  ownProps
) => {
  return {
    onClick: () => {
      dispatch({
        type: 'SET_VISIBILITY_FILTER',
        filter: ownProps.filter
      });
    }
  };
}
```

作为函数，应该返回一个对象，该对象的每个键值对都是一个映射，定义了 UI 组件的参数怎样发出 Action。

#### 2、Provider组件

`connect`方法生成容器组件以后，需要让容器组件拿到`state`对象，才能生成 UI 组件的参数。

一种解决方法是将`state`对象作为参数，传入容器组件。但是，这样做比较麻烦，尤其是容器组件可能在很深的层级，一级级将`state`传下去会很麻烦。

**React-Redux 提供`Provider`组件，可以作为顶层app的分发点，它只需要store属性就可以了。它会将`state`分发给所有被connect的容器组件，不管它在哪里，被嵌套多少层。**

**它接受store作为props，然后通过context往下传，这样react中任何组件都可以通过context获取store。**

```react
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import todoApp from './reducers'
import App from './components/App'

let store = createStore(todoApp);

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

上面代码中，`Provider`在根组件外面包了一层，这样一来，`App`的所有子组件就默认都可以拿到`state`了。

#### 3、React-Router 路由库

```react
const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      <Route path="/" component={App} />
    </Router>
  </Provider>
);
```



## Redux ToolKit

简称RTK。

官方网站：[链接](https://redux-toolkit.js.org/introduction/getting-started)

中文网站：[链接](https://redux-toolkit-cn.netlify.app/introduction/quick-start)

### Store Setup

#### 1、configureStore

正常情况下，调用creatStore()来创建一个Redux store，而Redux ToolKit 的 configureStore()覆盖了creatStore()来做同样的事。

```react
// createStore()
const store = createStore(counter)
// configureStore()
const store = configureStore({
    reducer: counter
})
```

#### 2、getDefaultMiddleware 

同上，见中间件部分

#### 3、Immutability Middleware



#### 4、Serializability Middleware



### Reducers and Actions

#### 1、createAction

createAction()接受一个action 类型字符串作为参数，并返回一个使用该类型字符串的 action creator 函数。

#### 2、createReducer

尽管你可以在一个 Redux reducer 中使用像 `if` 条件语句和循环这样的任何条件逻辑，最常见的实现是检查 `action.type` 字段然后为每个 action type 做特定的逻辑。

一个 reducer 也将提供一个初始化的状态值，如果 action 不是它所关心的则返回现有的状态。

 [`createReducer` 函数](https://redux-toolkit-cn.netlify.app/api/createReducer) ，它让使用"查找表"对象的方式编写 reducer，其中对象的每一个 key 都是一个 Redux action type 字符串，value 是 reducer 函数。我们可以直接使用它来替代现有的 `counter` 函数定义。由于我们需要使用 action type 字符串作为 key，所以我们可以使用 ES6 object "computed property" syntax 从 type 字符串变量来创建 key。

#### 3、createSlice

`createSlice` 函数允许我们提供一个带有 reducer 函数的对象，并且它将根据我们列出的 reducer 的名称自动生成 action type 字符串和 action creator 函数。

`createSlice` 返回一个 "分片" 对象，该对象包含生成的 reducer 函数作为一个名为 `reducer` 的字段，以及在一个名为 `actions` 的对象中生成的 action creator。

官方的计算器例子中

没有`createSlice`的情况下：

```react
function counter(state, action) {
	if (typeof state === 'undefined') {
       return 0
     }
    switch (action.type) {
        case 'INCREMENT':
            return state + 1
        case 'DECREMENT':
            return state - 1 // decrement的dom（document）不写了
        default:
            return state
    }
}

document.getElementById('increment')
    .addEventListener('click', function () {
	// 有type属性的Action
    store.dispatch({ type: 'INCREMENT' })
})
```

有`createSlice`的情况下：

```react
const RTK = window.RTK;

const counterSlice = RTK.createSlice({
  name: "counter",
  initialState: 0,
  reducers: {
    increment: state => state + 1,
    decrement: state => state - 1 // decrement的dom（document）不写了
  }
});
// increment() 和 decrement()
const { increment, decrement } = counterSlice.actions;

document
    .getElementById("increment")
    .addEventListener("click", function() {
    // increment()是一个带有 reducer 函数的对象，并且根据列出的 reducer 的名称自动生成 action type 字符串和 action creator 函数。
    	store.dispatch(increment());
	});
```

#### 4、createAsyncThunk

[官网链接](https://redux-toolkit.js.org/api/createAsyncThunk)

`createAsyncThunk`接受三个参数：字符串操作`type`值、`payloadCreator`回调和`options`对象。

- `type`
  - 为一个字符串，用于生成额外的 Redux 操作类型常量，表示异步请求的生命周期
  - `type`参数`'users/requestStatus'`将生成以下操作类型：
    - `pending`： `'users/requestStatus/pending'`
    - `fulfilled`： `'users/requestStatus/fulfilled'`
    - `rejected`： `'users/requestStatus/rejected'`
- `payloadCreator`
  - 一个回调函数，它应该返回一个包含一些异步逻辑结果的承诺。它也可以同步返回一个值。
  - `payloadCreator`将使用两个参数调用该函数：
    - `arg`: 单个值，包含在调度 thunk 动作创建者时传递给它的第一个参数。
    - `thunkAPI`：一个包含通常传递给 Redux thunk 函数的所有参数的对象，以及其他选项：
      - `dispatch`: Redux 存储`dispatch`方法
      - `getState`: Redux 存储`getState`方法
      - `extra`：设置时给 thunk 中间件的“额外参数”（如果可用）
      - `requestId`：自动生成的唯一字符串 ID 值，用于标识此请求序列
      - `signal`：一个[`AbortController.signal`对象](https://developer.mozilla.org/en-US/docs/Web/API/AbortController/signal)，可用于查看应用程序逻辑的另一部分是否已将此请求标记为需要取消。
      - `rejectWithValue(value, [meta])`：rejectWithValue 是一个实用函数,可以`return`（或`throw`）在您的Action creator 中使用已定义的有效负载和元返回拒绝的响应。
      - `fulfillWithValue(value, meta)`:fulfillWithValue 是一个实用函数，你可以`return`在你的Action creator 中`fulfill`使用一个值，同时有能力添加到`fulfilledAction.meta`.
- `options`
  - `condition(arg, { getState, extra } ): boolean | Promise<boolean>`：如果需要，可用于跳过有效负载创建者和所有Action分派的执行的回调。有关完整说明，请参阅[执行前取消](https://redux-toolkit.js.org/api/createAsyncThunk#canceling-before-execution)。
  - `dispatchConditionRejection`: 如果`condition()`返回`false`，默认行为是根本不会调度任何Action。如果您仍然希望在取消 thunk 时分派“拒绝”操作，请将此标志设置为`true`。
  - `idGenerator(arg): string`: 生成`requestId`请求序列时使用的函数。默认使用[nanoid](https://redux-toolkit.js.org/api/other-exports/#nanoid)，但您可以实现自己的 ID 生成逻辑。
  - `serializeError(error: unknown) => any``miniSerializeError`用您自己的序列化逻辑替换内部方法。
  - `getPendingMeta({ arg, requestId }, { getState, extra }): any`: 一个creator将被合并到`pendingAction.meta`字段中的对象的函数。
- return value
  - `createAsyncThunk`返回一个标准的 Redux thunk Action creator 。



### Others

#### 1、[createSelector](https://redux.js.org/usage/deriving-data-selectors#createselector-behavior)

```react
const selectA = state => state.a
const selectB = state => state.b
const selectC = state => state.c

const selectABC = createSelector([selectA, selectB, selectC], (a, b, c) => {
  // do something with a, b, and c, and return a result
  return a + b + c
})

// Call the selector function and get a result
const abc = selectABC(state)

// could also be written as separate arguments, and works exactly the same
const selectABC2 = createSelector(selectA, selectB, selectC, (a, b, c) => {
  // do something with a, b, and c, and return a result
  return a + b + c
})
```



## React - Query

[官网](https://tanstack.com/query/v4/docs/react/reference/useQuery)

### 基本

```react
// Create a client
const queryClient = new QueryClient()

function App() {
  return (
    // Provide the client to your App
    <QueryClientProvider client={queryClient}>
      {/* 添加devtools */}
      {props.development && <ReactQueryDevtools initialIsOpen={false} position='bottom-right' />}
      <Main />
    </QueryClientProvider>
  )
}
```

### useQuery

- useQuery接收一个唯一键和一个返回Promise的函数以及config `[queryKey, queryFn, config]`，如`posts`在内部用于在整个程序中重新获取数据、缓存和共享查询等
  - queryKey: 一个用于标识查询的键，可以是任意类型的值，但通常是一个字符串或一个数组。查询键会被哈希成一个稳定的键。当查询键改变时，查询会自动更新（除非 enabled 设置为 false）。
  - queryFn: 一个用于获取数据的函数，接收一个 QueryFunctionContext 参数，必须返回一个 promise，要么解析数据，要么抛出错误。数据不能是 undefined。如果没有定义默认的查询函数，这个参数是必需的。
  - options: 一个可选的对象，用来配置查询的行为，例如 retry, staleTime, cacheTime 等。

- isFetching 或者 status === 'fetching' 类似于isLoading，不过每次请求时都为true，所以使用isFetching作为loading态更好

- isLoading 或者 status === 'loading' 查询没有数据，正在获取结果中，只有“硬加载”时才为true，只要请求在cacheTime设定时间内，再次请求就会直接使用cache，即“isLoaindg = isFetching + no cached data”

- isError 或者 status === 'error' 查询遇到一个错误，此时可以通过 error 获取到错误

- isSuccess 或者 status === 'success' 查询成功，并且数据可用，通过 data 获取数据

- isIdle 或者 status === 'idle' 查询处于禁用状态

### 完整例子

```react
import {
  useQuery,
  useMutation,
  useQueryClient,
  QueryClient,
  QueryClientProvider,
} from 'react-query'
import { getTodos, postTodo } from '../my-api'

// 创建一个 client
const queryClient = new QueryClient()

function App() {
  return (
    // 提供 client 至 App
    <QueryClientProvider client={queryClient}>
      <Todos />
    </QueryClientProvider>
  )
}

function Todos() {
  // 访问 client
  const queryClient = useQueryClient()

  // 查询
  const query = useQuery('todos', getTodos)

  // 修改
  const mutation = useMutation(postTodo, {
    onSuccess: () => {
      // 错误处理和刷新
      queryClient.invalidateQueries('todos')
    },
  })

  return (
    <div>
      <ul>
        {query.data.map((todo) => (
          <li key={todo.id}>{todo.title}</li>
        ))}
      </ul>

      <button
        onClick={() => {
          mutation.mutate({
            id: Date.now(),
            title: 'Do Laundry',
          })
        }}
      >
        Add Todo
      </button>
    </div>
  )
}

render(<App />, document.getElementById('root'))
```

### fresh, fetching, stale 

为数据的新鲜度和有效期有关的概念

- fresh表示数据是最新的，不需要重新获取
- fetching表示数据正在被获取中，可能是第一次获取，也可能是重新获取

- stale表示数据已经过期，需要重新获取

react-query默认会在每次页面获得焦点时，检查数据是否过期，如果过期就重新获取。

可以通过配置staleTime来设置数据的有效期。你也可以通过useMutation来更新数据，并通过onSuccess回调来设置新的查询数据。

staleTime例子：

```react
const queryUsers = useQuery("gitUsers", () => {
  return fetch("https://api.github.com/search/users?q=joby").then((res) =>
    res.json()
  );
}, { staleTime: 5000 }); // 设置数据的有效期为5秒
```

### staleTime（不新鲜时间） 

默认0，可全局或单独配置，在此段时间内再次遇到相同key的请求，不会再去获取数据，直接从缓存中获取，isFetching也为false，如果设置为Infinity，则当前查询的数据只会获取一次，在整个网页的生命周期内缓存

### cacheTime（缓存时间） 

数据在内存中的缓存时间，默认5分钟，在不设置slateTime时，如果缓存期内遇到相同key的请求，虽然会直接使用缓存数据呈现UI，但还是会获取新数据，待获取完毕后切换为新数据，isFetching为true；如果某个queryKey未被使用时，这个query就会进入inactive状态，如果在cacheTime设定的时间内未被使用的话，这个query及其data就会被清除



## 轻量状态管理库 unstated-next

[github官网](https://github.com/jamiebuilds/unstated-next)

提供 `createContainer` 将自定义 Hooks 封装为一个可以提供状态和方法的 **数据对象**

利用 `useContext` 构造了 Provider 注入 和 组件获取获取 Store 这两个方法

### createContainer(useHook)

可以将任何 Hooks 包装成一个数据对象，这个对象有 `Provider` 与 `useContainer` 两个 API，其中 `Provider` 用于对某个作用域注入数据，而 `useContainer` 可以取到这个数据对象在当前作用域的实例。

对 Hooks 的参数也进行了规范化，我们可以通过 `initialState` 设定初始化数据，且不同作用域可以嵌套并赋予不同的初始化值：对 Hooks 的参数也进行了规范化，我们可以通过 `initialState` 设定初始化数据，且不同作用域可以嵌套并赋予不同的初始化值，官方例子：

```tsx
function useCounter(initialState = 0) {
  let [count, setCount] = useState(initialState);
  let decrement = () => setCount(count - 1);
  let increment = () => setCount(count + 1);
  return { count, decrement, increment };
}

const Counter = createContainer(useCounter);

function CounterDisplay() {
  let counter = Counter.useContainer();
  return (
    <div>
      <button onClick={counter.decrement}>-</button>
      <span>{counter.count}</span>
      <button onClick={counter.increment}>+</button>
    </div>
  );
}

function App() {
  return (
    <Counter.Provider>
      <CounterDisplay />
      <Counter.Provider initialState={2}>
        <div>
          <div>
            <CounterDisplay />
          </div>
        </div>
      </Counter.Provider>
    </Counter.Provider>
  );
}
```

### <Container.Provider>

`Provider` 就是对 `value` 进行了约束，**固化了 Hooks 返回的 value 直接作为** `value` **传递给** `Context.Provider` **这个规范。**

```tsx
function ParentComponent() {
  return (
    <Container.Provider>
      <ChildComponent />
    </Container.Provider>
  )
}
```

### <Container.Provider initialState>

`initialState` 用来初始化数据

```tsx
function useCustomHook(initialState = "") {
  let [value, setValue] = useState(initialState)
  // ...
}

function ParentComponent() {
  return (
    <Container.Provider initialState={"value"}>
      <ChildComponent />
    </Container.Provider>
  )
}
```

### Container.useContainer()

 `useContainer` 就是对 `React.useContext(Context)` 的封装。作用为使用数据。

```tsx
function ChildComponent() {
  let input = Container.useContainer()
  return <input value={input.value} onChange={input.onChange} />
}
```

### useContainer(Container)

```tsx
import { useContainer } from "unstated-next"

function ChildComponent() {
  let input = useContainer(Container)
  return <input value={input.value} onChange={input.onChange} />
}
```

### 和useContext对比

```diff
- import { createContext, useContext } from "react"
+ import { createContainer } from "unstated-next"

  function useCounter() {
    ...
  }

- let Counter = createContext(null)
+ let Counter = createContainer(useCounter)

  function CounterDisplay() {
-   let counter = useContext(Counter)
+   let counter = Counter.useContainer()
    return (
      <div>
        ...
      </div>
    )
  }

  function App() {
-   let counter = useCounter()
    return (
-     <Counter.Provider value={counter}>
+     <Counter.Provider>
        <CounterDisplay />
        <CounterDisplay />
      </Counter.Provider>
    )
  }
```

### 多层Container.Provider嵌套问题

```tsx
const containers: any[] = []; // 👈数组里填要合并的Containers
function Composed(props: any) {
    return containers.reduce((children, Container) => {
      return <Container.Provider>{children}</Container.Provider>;
    }, props.children);
  };
}
```

完整例子：

```tsx
// compose.tsx
import React from "react";

export function compose(...containers: any[]) {
  return function Composed(props: any) {
    return containers.reduceRight((children, Container) => {
      return <Container.Provider>{children}</Container.Provider>;
    }, props.children);
  };
}

// main.tsx
import React from "react";
import ReactDOM from "react-dom";
import { GeneralContainer } from "./containers/GeneralContainer";
import { UserContainer } from "./containers/UserContainer";
import { PlanContainer } from "./containers/PlanContainer";
import { compose } from "./containers/compose";
import App from "./App";

const Composed = compose(GeneralContainer, UserContainer, PlanContainer);

ReactDOM.render(
  <Composed>
    <App />
  </Composed>,
  document.getElementById("app")
);

```



## 其他知识补充

### 不渲染问题

- 组件render了一个对象，当state确定更新，但视图没有更新

  - 原因：对象是引用传递，对于react来说他的值都是地址，因为没有重新赋值（地址没有改变），所以react会认为仍是之前的元素。

  - 示例代码：

    ```react
    const obj = this.state.obj;
    // 正确方式：
    const obj = [...this.state.obj];
    ```

- 组件渲染正常，视图显示异常

  - 原因可能是key的问题
  - key应该是唯一的，在兄弟节点之间必须唯一，但全局不需要唯一。
  - 一般使用后端传过来的id，记得避免使用index或者random，index会导致bug，random和没有使用的效果是一样的。

- 深拷贝浅拷贝问题

  - 会导致地址改变的一些行为，也会导致渲染或者视图异常
  - 示例代码：

  ```react
  eles.sort();
  // 以上这行会导致地址被改变，需要先解构，如下写法
  eles = [...eles].sort();
  // 或
  eles = eles.slice().sort();
  ```



### 使对象不可变的小技巧

- `const newState = Object.assign({}, state, {foo:123});`
- 解构
  - `const newState = {...state, foo:123}`



### JS & TS补充

#### setInterval() 和 setTimeout() 的区别

`setTimeout(expression, delayTime)`，在DelayTime过后，将执行一次Expression；

`setInterval(expression, delayTime)`，每个DelayTime，都会执行一次Expression。setInterval为不停的循环执行表达式。

```react
document.getElementById('incrementAsync')
    .addEventListener('click', function () {
    // 这个地方如果使用setTimeout，则加一次便停止了。如果使用setInterval，则会每隔一秒循环INCREMENT。
    setTimeout(function () {
        store.dispatch({ type: 'INCREMENT' })
    }, 1000)
})
```

#### 数据类型问题

any: 即任何类型，ts 中只要不指定类型注解即可。

`{}: any`

`{} as any`

关于对象的 JS 和 TS 对比写法：

```react
let obj: object = { x: 1, y: 2 }
obj['x'] = 2
```

```react
let obj: {x: number, y: number} = {x: 1, y: 2} // 需要声明xy的属性为number
obj.x = 2
```

#### 三元运算符(?.)补充

这个语句为：判断a是不是为空（a?），如果不是，return a.p1(a?.p1), 如果是空（else：??），return " "。

```react
let b = a?.p1 ?? " ";
// 相当于以下的简化
let b = a?.p1 ? a?.pl : " ";
```

检查严格时，如果不判断，会报错：

`Uncaught (in promise) TypeError`

`Cannot read properties of undefined (reading 'value').`

#### object内的缩写问题

```react
nodeInfos.push({id: result[i][j], x: xRight, y: y});
```

其中 `y: y` 可以缩写成 `y` ，当值是一样的时候可以缩写，否则要声明 `x: xRight`，不然会覆盖改变x变成类似xRight: xRight这种。

#### return内的组件判断问题

```react
<>
	{isTrue && (
		<xxx />
	)}
</>
```

#### 循环Object.keys

通过Object.keys()方法会返回一个由一个给定对象的自身可枚举属性组成的数组。

```react
// 循环
const viewPlotMap: { [key: string]: NetworkPlot } = {};
Object.keys(viewPlotMap).forEach((plot) => {
    // doSomething...
});
```

#### StrictMode严格模式

开发模式下的严格模式会导致react重复渲染一次。

```react
<React.StrictMode>
```

#### CSS相关

鼠标变成禁止符号：cursor:"not-allowed"


### Base64编码

一个String之类的东西通过base64编码，成为一个image.

`toDataURL()`



### Echart做图


```react
option = {
	series: datas.map(function(item, idx) {
		return {
            // 类型
			type: 'graph',
            // 力引导布局
			layout: 'force', // 布局参数：none/force/circular
            // 单个元素开启平移或缩放
            roam: true,
            // 是否可以拖拽
            draggable: true,
            // 动画
			animation: false,
			data: item.nodes,
            // 位置
			left: (idx % 4) * 25 + '%',
			top: Math.floor(idx / 4) * 25 + '%',
			width: '25%',
			height: '25%',
			force: {
				// initLayout: 'circular'
				// gravity: 0
				repulsion: 60,
				edgeLength: 2 // 线的长度
			},
			edges: item.edges.map(function(e) {
				return {
					source: e[0],
					target: e[1]
				};
			})
		};
	})
};

// 鼠标拖拽
myChart.on('mouseup', function(params) {
  var option = myChart.getOption();
  option.series[0].data[params.dataIndex].x = params.event.offsetX;
  option.series[0].data[params.dataIndex].y = params.event.offsetY;
  option.series[0].data[params.dataIndex].fixed = true;
  myChart.setOption(option);
});

// 显示
return(
	<div>
    	<ReactECharts 
    		option = {option}
    	>
    <div>
)
```



### Cytoscape.js做图

#### Install

**npm环境下：**

```bash
npm install cytoscape
```

**yarn环境下：**

配置yarn环境，-g全局。

```bash
npm i -g yarn
```

yarn环境配置：

```bash
yarn add cytoscape
```

如果已经配置yarn，需要加入白名单，放入公司的私有storage里，从公司的storage转接过来：

在`.yarnrc.yml`下：

```react
npmRegistryServer: http://192.168.30.248:8081/repository/yarn-group/

unsafeHttpWhitelist: ["192.168.30.248"]
```

yarn设置白名单指令：

```bash
yarn config set -- json unsafeHttpWhitelist ["192.168.30.248"]
```

#### [Including Cytoscape.js](https://js.cytoscape.org/#getting-started/including-cytoscape.js)

```react
import cytoscape from "./cytoscape.esm.min.js";
```

```react
require(['cytoscape'], function(cytoscape){
  // ...
});
```

**run program:**

`package.json` show npm Scripts

#### Events

##### [device events](https://js.cytoscape.org/#events/user-input-device-events)

- `mouseover` : when the cursor is put on top of the target
- `mouseout` : when the cursor is moved off of the target

##### [custom events](https://js.cytoscape.org/#events/collection-events)

#### [Layouts](https://js.cytoscape.org/#layouts)

- [random](https://js.cytoscape.org/#layouts/random)
- [circle](https://js.cytoscape.org/#layouts/circle)
- [concentric](https://js.cytoscape.org/#layouts/concentric)
- core
- ...

## 附加

road map

![roadmap](https://github.com/AaronPhantomhive/little-storage-pubic/blob/main/React/roadmap.png)
