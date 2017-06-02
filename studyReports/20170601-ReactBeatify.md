# 2017-06-01 优化React写法和性能

## 几种更好的写法
### 在class中初始化state（es7）
```javascript
export default class Comp extends Component {
  state = { expanded: false }
```
### 设置默认props及props类型
```javascript
export default class Comp extends Component {

  static propTypes = {
    model: React.PropTypes.object.isRequired,
    title: React.PropTypes.string
  }

  static defaultProps = {
    model: {
      id: 0
    },
    title: 'Your Name'
  }
```
> 注：propTypes和defaultProps是静态属性，在组件代码中声明尽可能高。其他开发人员可作为api文档来查看。


### 函数最快绑定this的方法
```javascript
export default class Comp extends Component {
  handleSubmit = (e) => {
    e.preventDefault()
    this.props.model.save()
  }
```

### 减少Reconciliation
> **一定要记住，只要 props（或 state）的值不等于之前的值，React 就会触发重新渲染。**

* 避免传递新的闭包到子组件
每次父组件渲染时，都会相当于创建一个新的函数

```javascript
<input
	type="text"
	value={model.name}
	// onChange={(e) => { model.name = e.target.value }}
	// like under, it's so bad!（忍不住飙英语）, 使用下面的方式：
	onChange={this.handleChange}
	placeholder="Your Name"/>

```
* 不要在props里直接使用对象或数组字面量
相当于调用了 Object.create() 和 new Array()，一个新的引用。经常出现于行内样式中。


```javascript
/* Bad */
// 每次渲染时都会为 style 新建一个对象字面量
render() {
  return <div style={ { backgroundColor: 'red' } }/>
}

/* Good */
// 在组件外声明
const style = { backgroundColor: 'red' };

render() {
  return <div style={ style }/>
}
```

* 注意兜底值字面量
有时我们会在 render 函数中创建一个兜底的值来避免 undefined 报错。在这些情况下，最好在组件外创建一个兜底的常量而不是创建一个新的字面量。


```javascript
/* Bad */
render() {
  let thingys = [];
  // 如果 this.props.thingys 没有被定义，一个新的数组字面量会被创建
  if( this.props.thingys ) {
    thingys = this.props.thingys;
  }

  return <ThingyHandler thingys={ thingys }/>
}

/* Bad */
render() {
  // 这在功能上和前一个例子一样
  return <ThingyHandler thingys={ this.props.thingys || [] }/>
}

/* Good */

// 在组件外部声明
const NO_THINGYS = [];

render() {
  return <ThingyHandler thingys={ this.props.thingys || NO_THINGYS }/>
}
```
### 其他几个小原则
* 不要在render进行复杂运算和操作（render主攻已经够忙了，别烦它了好么）
* 避免在组件的state上存储一些容易计算（通过简单的字符串拼接或基本的算数运算从 props 派生出来）的值，不要搞乱state的使用
* 尽可能的保持Props和State精简，请听天哥劝，适时的使用connect。
	为了将值传给子组件而将一个大的、复杂的对象或者很多独立的 props 传递给一个组件，这是我们的经常行为。殊不知，会导致很多不必要的组件渲染，还会增加开发复杂性。
* 请尽量使用 helper/util 模块的纯函数或者静态类方法，如果这样的话会减少渲染负担（明显静态方法比实例方法效率高）。

## 高阶
### 使用装饰器Decorator(es7)
> Decorator就是使用高阶组件模式（高阶组件是什么鬼？可看[天哥牛文](http://wiki.inwaimai.baidu.com/pages/viewpage.action?pageId=4277071)），它并不改变原有组件，而是通过编写一个包裹组件（装饰器），在包裹组件中书写一些额外的功能来增强原有组件。举个例子，当切换React路由时，动态更改title属性的装饰器。

```javascript
//假如有这么一个页面组件，用于显示用户资料的，当从Home组件进去到这个组件时
//希望title从“Home Page”变成“Profile Page”
//注意这里隐形传入了组件，语法类似setTitle('Profile Page')(Profile)
@setTitle('Profile Page')
class Profile extends React.Component {
    //....
}

//开始定义装饰器
//title参数对应上面的“Profile Page”字符串
//WrappedComponent参数对应上面的Profile组件
//然后在组件加载完修改了title，再返回一个新组件，是不是很像高阶组件呢
const setTitle = (title) => (WrappedComponent) => {
   return class extends React.Component {
      componentDidMount() {
          document.title = title
      }
      render() {
         return <WrappedComponent {...this.props} />
      }
   }
}
```
> 注：装饰器通过灵活、可读的方式来修改组件功能，应用场景广泛，比如lazyload、我们的多时间段组件。也可配合[mobox](https://github.com/mobxjs/mobx)使用。


### 函数式组件
> 十分简单易读，适用于无状态组件


* 声明propTypes


```javascript
import React from 'react'
const compProps = {
  onExpand: React.PropTypes.func.isRequired,
  expanded: React.PropTypes.bool
}
Comp.propTypes = compProps
```
* 解构Props和设置defaultProps

```javascript
function Comp({ onExpand, expanded = false, children }) {
  return (
    <div style={ expanded ? { height: 'auto' } : { height: 0 } }>
      {children}
      <button onClick={onExpand}>Expand</button>
    </div>
  )
}
```
* 装饰器包裹

```javascript
// 比如装饰器observer
export default observer(ExpandableForm)
```

### React.PureComponent
> PureComponent声明了它自己的 shouldComponentUpdate() 方法，它自动对当前的和下一次的 props 和 state 做一次浅对比。如此看来，与immutable简直是完美的结合。
请谨记两点：


* 纯组件忽略重新渲染时，不仅会影响它本身，而且会影响它的所有子元素，所以，使用PureComponent的最佳情况就是展示组件，它既没有子组件，也没有依赖应用的全局状态。
* 在纯组件有子组件的时候，所有基于 this.context 改变的子组件， 在 this.context 改变时， 将不会重新渲染 ，除非在父组件中声明 contextTypes 


## 性能陷阱
### setTimeout() 和 setInterval()
> 在 React 组件中使用 setTimeout() 或者 setInterval() 要十分小心，要尽量减少使用，正所谓少用少出错。


* 如果你不听话非要用，请遵守以下两个原则。
	1. 不要设置过短的时间间隔。
		当心那些小于 100 ms 的定时器，他们很可能是没意义的。如果确实需要一个更短的时间，可以使用 window.requestAnimationFrame()。
	2. 保留对这些函数的引用，并且在 unmount 时取消或者销毁他们。
		setTimeout() 和 setInterval() 都要返回一个延迟函数的引用，并且在需要的时候可以销毁它们。由于这些函数是在全局作用域执行的，他们不在乎你的组件是否存在，这会导致报错甚至程序卡死。当然， 对 window.requestAnimationFrame() 来说也是如此
	
	正确的姿势如下：


```javascript
// timeout/interval
compnentDidMount() {
 this._timeoutId = setTimeout( this.doFutureStuff, 1000 );
 this._intervalId = setInterval( this.doStuffRepeatedly, 5000 );
}
componentWillUnmount() {
 /*
   高级提示：如果操作已经完成，或者值未被定义，这些函数也不会报错
 */
 clearTimeout( this._timeoutId );
 clearInterval( this._intervalId );
}


// 动画循环
componentDidMount() {
  this.startLoop();
}

componentWillUnmount() {
  this.stopLoop();
}

startLoop() {
  if( !this._frameId ) {
    this._frameId = window.requestAnimationFrame( this.loop );
  }
}

loop() {
  // 在这里执行循环工作
  this.theoreticalComponentAnimationFunction()

  // 设置循环的下一次迭代
  this.frameId = window.requestAnimationFrame( this.loop )
}

stopLoop() {
  window.cancelAnimationFrame( this._frameId );
  // 注意: 不用担心循环已经被取消
  // cancelAnimationFrame() 不会抛出异常
}
```


* 解决这个问题，其实可以用 [react-timeout](https://www.npmjs.com/package/react-timeout) 这个 NPM 包，它提供了一个可以自动处理上述内容的高阶组件（将 setTimeout/setInterval 等功能添加到包装组建的 props 上）。


### 未去抖频繁触发的事件
* 一般需求：只需去抖
	使用Lodash的_.debounce 方法或用 debounce 的npm包
* 高阶需求：去抖的同时，也要立即反馈 
	在第一次事件触发时启动 requestAnimationFrame() 循环。然后可以使用 [lodash.debounce()](https://lodash.com/docs#debounce) 并且将 trailing 这个配置项设为 true（这意味着该功能只在频繁触发的事件流结束后触发）来取消对值的监听


```javascript
class ScrollMonitor extends React.Component {
  constructor() {
    this.handleScrollStart = this.startWatching.bind( this );
    this.handleScrollEnd = debounce(
      this.stopWatching.bind( this ),
      100,
      { leading: false, trailing: true } );
  }

  componentDidMount() {
    window.addEventListener( 'scroll', this.handleScrollStart );
    window.addEventListener( 'scroll', this.handleScrollEnd );
  }

  componentWillUnmount() {
    window.removeEventListener( 'scroll', this.handleScrollStart );
    window.removeEventListener( 'scroll', this.handleScrollEnd );

    //确保组件销毁后结束循环
    this.stopWatching();
  }

  // 如果循环未开始，启动它
  startWatching() {
    if( !this._watchFrame ) {
      this.watchLoop();
    }
  }

  // 取消下一次迭代
  stopWatching() {
    window.cancelAnimationFrame( this._watchFrame );
  }

  // 保持动画的执行直到结束
  watchLoop() {
    this.doThingYouWantToWatchForExampleScrollPositionOrWhatever()

    this._watchFrame = window.requestAnimationFrame( this.watchLoop )
  }

}
```
### 密集CPU任务线程阻塞
比如非常复杂的数学计算、迭代非常大的数组、使用 File api 进行文件读写、利用`<canvas>`对图片进行编码解码等等。
在以上情况下，最好使用 Web Worker 将这些功能移到另一个线程上，这样我们的主渲染线程可以保持顺滑。


## 参考链接
* [Our Best Practices for Writing React Components](http://www.tuicool.com/articles/7nemyi2)
* [光速 React](https://juejin.im/post/591ad6b7128fe1005ce1123a)

* [在React.js中使用PureComponent的重要性和使用方式](http://www.open-open.com/lib/view/1484466792204)
