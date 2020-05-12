---
title: React面试题集锦
index_img: /img/react/react.jpg
tags: [react]
category: 技术
abbrlink: f94fb1be
date: 2020-05-12 20:19:00
---

持续更新~~

### 1.redux中间件的原理是什么？

> 中间件是指action和store之间，用来改装dispatch
原来的action是一个对象，那么通过redux-thunk这个中间件就可以将action改装成一个函数然后返回一个对象。

![redux](/img/react/redux.jpg)

### 2.你会把数据统一放到redux中管理，还是共享数据放在redux中管理？

应当是所有的数据都放到redux中管理。
在react中存储数据有state，props，redux这三种方式。如果部分放在redux，部分放在state或者props里面，那么对于数据的维护和出现问题的排查是不利的。

### 3.componentWillReceiveProps的调用时机？ `已废弃`
当props发生变化时执行，初始化render时不执行

### 4.react性能优化的最佳实践？
+ 减少重新render的次数  ---shouldComponentUpdate/PureComponent
+ 减少计算量，主要是减少重复计算。对于函数式组件来说，每次render都会重新从头开始执行函数调用 ---React.memo

### 5.虚拟dom是什么？为什么虚拟dom会提升性能？
> 虚拟dom就是一个js对象。
react通过数据改变来比对前后虚拟dom之间的差异，然后用实际更改的内容更新真实dom。

### 6.webpack中， 是借助loader完成的JSX代码的转化，还是babel?
babel --- preset-react

### 7.调用setState后，发生了什么？setState一般怎么用，传一个对象还是函数？
在代码中调用了setState之后，React会将传入的参数对象与组件当前的状态合并，然后触发调和过程，经过调和过程，react会以相对高效的方式根据新的状态构建react元素树并开始重新渲染UI界面。在得到元素树之后，会自动计算新的树与老的树的节点差异，然后根据差异对界面进行最小化重渲染。在差异计算中，react能够相对精确的知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染。
官方推荐：
```js
this.setState((preState) => ({
	name:'newValue'
}))
```

### 8.setState是异步的，这个点你在什么时候遇到过坑？
一般是在调用this.setState()之后访问修改的值，取到的还是之前的结果，并不是更新后的。
若要访问更新后的需要在第二个回调函数里面访问出来
```js
this.setState((preState)=>({
	name:'xxx'
}),()=>{
    console.log(this.state.name);
}）
```

### 9.refs的作用是什么，你在什么业务场景下使用过refs?
+ 操作dom，比如渲染图片后，需要获取图片的宽高。需要管理焦点，文本选择或者媒体播放，触发强制动画，集成第三方DOM库
+ 操作组件，向组件添加属性或方法

### 10.ref是一个函数，有什么好处？
+ react可以更优雅的完成对组件/元素销毁时的变量回收，你只要这么用，react在销毁组件时，很方便的就可以被清理变为null。
+ 可以做更多的操作，例如：让父组件直接获取子组件的Dom，而如果你让ref是一个字符串，实现这个功能是不可能的
```js
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}
class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```

### 11.高阶组件HOC你是怎么理解的，它的本质是一个什么东西？
> 高阶组件是一个以组件为参数并返回一个新组件的函数。
最常见的可能是 Redux 的 connect 函数
+ 代码重用，逻辑和引导抽象
+ 渲染劫持
+ 状态抽象和控制
+ Props 控制

### 12.受控组件和非受控组件的区别？
受控组件：这个组件的改变受控于数据的变化，数据变了，页面跟着变化。例如：
```js
class Demo1 extends Component {
    constructor(props) {
        super(props);
        this.state = {
            value: props.value
        }
    }

    handleChange(e) {
        this.setState({
            value: e.target.value
        })
    }

    render() {
        return (
            <input value={this.state.value} onChange={e => this.handleChange(e)}/>
        )
    }
}
```

非受控组件：组件的状态不受react控制
```js
class Demo1 extends Component {
    render() {
        return (
            <input />
        )
    }
}
```

### 13.this指向问题你一般是怎么解决的？
+ 箭头函数
+ bind this放在constructor里面


### 14.函数组件怎么做性能优化？
用React.memo包装函数组件，类似于类组件的shouldComponentUpdate

### 15.那个生命周期里发送ajax?
> componentDidMount
这个方法会在组件第一次“挂载”(被添加到 DOM)时执行，在组件的生命周期中仅会执行一次。更重要的是，你不能保证在组件挂载之前 Ajax 请求已经完成，如果是这样，也就意味着你将尝试在一个未挂载的组件上调用 setState，这将不起作用。在 componentDidMount 中发起网络请求将保证这有一个组件可以更新了。

### 16.react 的ssr的原理是什么？
> 借助虚拟dom完成操作

### 17.react,jquery,vue是否有可能共存在一个项目中？
可以，怎么共存？
```html
<body>
    <div id="jquery"></div>    //这个div用于jquery操作
    <div id="react"></div>		//这个用于react
 	<div id="vue"></div>		//这个用于vue
 </body>
<script>
	ReactDom.render(<App />,document.getElementById('react'));
    let vue = new Vue({
    	el:"#vue",
        data(){
        	return {
            	
            }
        }
    })
</script>
```

### 18.组件是什么？类是什么？类被编译成什么？
组件：指的是页面的一部分
类：本质就是一个构造函数
被编译为：构造函数

### 19.如何避免ajax数据重新获取
数据采用redux和react-redux来管理

### 20.react-router4的核心思想是什么？和3有什么区别
核心思想是：路由也是组件 <Link></Link> <Route></Route>
3是路由配置，4是路由即组件

### 21.reselect是做什么使用的？
这个库类似于vue的computed

### 22.react-router的基本原理，hashHistory,browserHistory
+ hashHistory 前端直接搞定
+ browserHistory 需要后台在服务器进行配置

### 23.什么情况下使用异步组件
reloadable库，路由懒加载或者叫按需加载

### 24.xss攻击在react中如何防范？
react默认防范，我们慎用dangerouslySetInnerHTML
```js
export default class TodoInput extends React.Component {
    constructor(props){
        super(props)
        this.state = {
        	title:'<h1>解析html</h2>'
        }
    }
	render(){
    	return(
        	<Fragment>
            	<div dangerouslySetInnerHTML = {{__html:this.state.title}}></div>
            </Fragment>
        )
    }
}
```

### 25.react中keys的作用是什么？
> Keys 是 React 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。

```js
render () {
  return (
    <ul>
      {this.state.list.map(({item, key}) => {
        return <li key={key}>{item}</li>
      })}
    </ul>
  )
}
```
在开发过程中，我们需要保证某个元素的 key 在其同级元素中具有唯一性。在 React Diff 算法中 React 会借助元素的 Key 值来判断该元素是新创建的还是被移动而来的元素，从而减少不必要的元素重渲染。此外，React 还需要借助`Key`值来判断元素与本地状态的关联关系，因此我们绝不可忽视转换函数中 Key 的重要性。

### 26.生命周期函数
1.初始化阶段：
+ `getDefaultProps`:获取实例的默认属性 `已废弃`
+ `getInitialState`:获取每个实例的初始化状态  `已废弃`
+ `componentWillMount`：组件即将被装载、渲染到页面上  `已废弃`
+ `render`:组件在这里生成虚拟的 DOM 节点
+ `componentDidMount`:组件真正在被装载之后

2.运行中状态：
+  `componentWillReceiveProps`:组件将要接收到属性的时候调用    `已废弃`
+ `static getDerivedStateFromProps()`
    > 是一个静态方法
    + 从props中获取衍生的state   
    + 它必须返回一个对象用于更新state,返回null则不更新任何内容
    + 没有this
    + 会在调用render之前调用，并且在初始挂载和后续更新中都会被调用。
+ `shouldComponentUpdate(nextProps, nextState)`:组件接受到新属性或者新状态的时候（可以返回 `false`，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行了）
    + 当props和state发生改变时，shouldComponentUpdate会在渲染之前被调用。返回值默认为`true`
    + 首次渲染和调用`forceUpdate()`时不会调用该方法。
    + `shouldComponentUpdat`e 只作为性能优化而存在。PureComponent 会对props和state进行浅层比较，不要用JSON.stringify()进行比较
    + 返回 `false` 不会调用`render()` `componentDidUpdate()` 并不会阻止子组件在state更改时重新渲染
+ `componentWillUpdate`:组件即将更新不能修改属性和状态  `已废弃`
+ `render`:组件重新描绘 
+ `componentDidUpdate(prevProps,prevState,snapshot)`:组件已经更新
    > https://zh-hans.reactjs.org/docs/react-component.html#componentdidupdate
    + `componentDidUpdate`会在更新后立即被调用，首次渲染不会调用
    + 当组件更新后，可以在此次对DOM进行操作。如果对前后props进行了比较，也可以进行网络请求
    ```js
        componentDidUpdate(prevProps) {
            // 典型用法（不要忘记比较 props）：
            if (this.props.userID !== prevProps.userID) {
                this.fetchData(this.props.userID);
            }
        }
    ```

    + 也可以在`componentDidUpdate`中调用`setState()`,但他必须要被包含在条件判断语句内，否则会导致死循环
    + 第三个参数如果`getSnapshotBeforeUpdate`未返回则是 `undefined`

3.销毁阶段：
+ `componentWillUnmount`:组件即将销毁
    + `componentWillUnmount` 会在组件销毁之前调用。在此方法中执行必要的清理操作。（例如：清除定时器，取消网络请求，解除绑定事件，...）
    + 不应该使用setState,因为组件将永远不会被渲染。

![react生命周期](/img/react/lifecycle.jpg)