---
title: Taro 采坑指南
index_img: /img/taro.png
tags: [Taro,小程序]
category: 技术
abbrlink: f94fb1be
date: 2020-03-12 16:00:00
---

### 在Taro中如何正确给自定义组件传递样式类
需要利用 externalClasses 定义段定义若干个外部样式类。这个特性从小程序基础库版本 1.9.90 开始支持。

```js
/* CustomComp.js */
export default class CustomComp extends Component {
  static externalClasses = ['my-class']

  render () {
    return <View className="my-class">这段文本的颜色由组件外的 class 决定</View>
  }
}

```
```js
/* MyPage.js */
export default class MyPage extends Component {
  render () {
    return <CustomComp my-class="red-text" />
  }
}

```

```scss
/* MyPage.scss */
.red-text {
  color: red;
}

```

注意：externalClasses 需要使用 短横线命名法 (kebab-case)，而不是 React 惯用的 驼峰命名法 (camelCase)。否则无效。

### 父组件的样式类完全影响子组件
使用外部样式类可以让组件使用指定的组件外样式类，如果希望组件外样式类能够完全影响组件内部，可以将组件构造器中的 options.addGlobalClass 字段置为 true。这个特性从小程序基础库版本 2.2.3 开始支持。
```js
/* CustomComp.js */
export default class CustomComp extends Component {
  static options = {
    addGlobalClass: true
  }

  render () {
    return <View className="red-text">这段文本的颜色由组件外的 class 决定</View>
  }
}
```
```css
/* 组件外的样式定义 */
.red-text {
  color: red;
}
```

### 在Taro中给子组件传递属性时，不能使用 ... 拓展操作符
```js
const props = {firstName: 'Plus', lastName: 'Second'}
return <Greeting {...props} /> //错误
return <Greeting firstName="Plus" lastName="Second" /> //正确
```

### 在Taro中如何阻止事件冒泡
在 Taro 中你不能使用 `catchEvent` 的方式阻止事件冒泡。你必须明确的使用 `e.stopPropagation`。例如，阻止事件冒泡你可以这样写：
```js
class Toggle extends Component {
  constructor (props) {
    super(props)
    this.state = {isToggleOn: true}
  }

  onClick = (e) => {
    e.stopPropagation()
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }))
  }

  render () {
    return (
      <button onClick={this.onClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    )
  }
}
```

### Taro中事件处理的传参方式
1.使用 `bind` 方式向监听函数传参，在类组件中定义的监听函数，事件对象 `e` 要排在所传递参数的后面。
```js
class Popper extends Component {
  constructor () {
    super(...arguments)
    this.state = { name:'Hello world!' }
  }

  // 你可以通过 bind 传入多个参数
  preventPop (name, test, e) {    //事件对象 e 要放在最后
    e.stopPropagation()
  }

  render () {
    return <Button onClick={this.preventPop.bind(this, this.state.name, 'test')}></Button>
  }
}

```

2.使用`匿名函数`
直接写匿名函数不会打乱原有监听函数的参数顺序。
注意：在各小程序端，使用匿名函数，尤其是在 `循环中` 使用匿名函数，比使用 `bind` 进行事件传参占用更大的内存，速度也会更慢。
```js
class Popper extends Component {
  constructor () {
    super(...arguments)
    this.state = { name: 'Hello world!' }
  }

  render () {
    const name = 'test'
    return (
      <Button onClick={(e) => {
        e.stopPropagation()
        this.setState({
          name
        })
      }}>
        {this.state.name}
      </Button>
    )
  }
}
```

3.[柯里化](https://zh.wikipedia.org/wiki/%E6%9F%AF%E9%87%8C%E5%8C%96)
```js
class Title extends Component{

  handleClick = (index) => (e) => {
    e.stopPropagation()
    this.setState({
      currentIndex: index
    })
  }

  render() {
    const { currentIndex } = this.props;
    return (
      {/* 调用 `this.handleClick(currentIndex)` 会返回一个函数，这个函数可以访问到 `currentIndex` 同时也能满足 `onClick` 的签名 */}
      <View onClick={this.handleClick(currentIndex)}>
      </View>
    )
  }
 }
```

4.函数式组件传递方式
在函数式组件中，事件传参可以传入`事件的引用`也可以传入`匿名函数`，以下是函数式组件配合 `useCallback` 的一个例子：
```js
const App = () => {
  const [c1, setC1] = useState(0);
  const [c2, setC2] = useState(0);
  const [c3, setC3] = useState(0);

  const increment = c => c + 1

  // 只有 useCallback 对应的 c1 或 c2 的值改变时，才会返回新的函数
  const increment1 = useCallback(() => setC1(increment), [c1]);
  const increment2 = useCallback(() => setC2(increment), [c2]);

  return (<View>
    <Text> Counter 1 is {c1} </Text>
    <Text> Counter 2 is {c2} </Text>
    <Text> Counter 3 is {c3} </Text>
    <View>
      <Button onClick={increment1}>Increment Counter 1</Button>
      <Button onClick={increment2}>Increment Counter 2</Button>
      <Button onClick={() => setC3(increment)}>Increment Counter 3</Button>
    </View>
  </View>)
}
```

5.任何组件的事件传递都要以 `on` 开头
在 v1.3.0-beta.0 之后，自定义组件间的事件传递可以不用 `on` 开头，但内置组件的事件依然是以 `on` 开头的，为了一致性我们仍然推荐你以 `on` 开头命名你的事件。
```js
const element = <View onClick={this.onTag} />
const element2 = <Input onFocus={this.onFocus} />
const element3 = <CustomElement onAnimationEnd={this.props.onAnimationEnd} />

```

### Taro中巧用逻辑运算符 && 和 三元运算符 以及 枚举 来实现条件渲染
```js
class LoginStatus extends Component {
  render () {
    const isLoggedIn = this.props.isLoggedIn

    return (
      <View>
        {isLoggedIn && <Text>已登录</Text>}
        {!isLoggedIn && <Text>未登录</Text>}

        {isLoggedIn
            ? <Text>已登录</Text>
            : <Text>未登录</Text>
        }

      </View>
    )
  }
}
```

```js
function Loading (props) {
  const { loadingText, LOADING_STATUS, loadingStatus, onRetry } = props
  return (
    <View className='loading-status'>
      {
        {
          'loading': loadingText,
          'fail': <View onClick={onRetry}> 加载失败, 点击重试 </View>,
          'no-more': '没有更多了'
        }[loadingStatus] /** loadingStatus 是 `loading`、`fail`、`no-more`  其中一种状态 **/
      }
    </View>
  )
}
```

### Taro中循环渲染 原生小程序组件 需要指定taroKeys
`taroKey` 适用于循环渲染原生小程序组件，赋予每个元素唯一确定标识，转换为小程序的 `wx:key`。
```js
const numbers = [...Array(100).keys()] // [0, 1, 2, ..., 98, 99]
const listItems = numbers.map((number) => {
  return (
    // native component
    <g-list
      taroKey={String(number)}
      className='g-list'
    >
    我是第 {number + 1} 个数字
    </g-list>
  )
})
```