## 2016-10-25 React组件性能优化


## 一、性能监控工具
> 使用react的perf工具,得出该组件的一系列性能数据
 
**使用方法：**
1. 第一步：在react中引入react-addons-perf
```javascript
import Perf from 'react-addons-perf'

Perf.start();
....your react code
Perf.stop();
```
2. 第二步：使用API方法，通过console面板或者下载chrome 插件React Perf来调试
	* `Perf.printWasted()`输出无意义的render
		用法：不断优化，将该列表优化至为空
	* `Perf.printInclusive()`输出渲染各个组件的总体时间
		用法：找出页面渲染的性能瓶颈
 
## 二、性能问题 
### (一)减少不必要的虚拟DOM对比和render

起因：在初始化渲染的时候会调用根组件下的所有组件的render方法进行渲染。而当我们要更新某个子组件的时候，理想状态是只调用关键路径上组件的render，但是react的默认做法是调用所有组件的render，再对生成的虚拟DOM进行对比，如不变则不进行更新。这样的render和虚拟DOM的对比明显是在浪费。
 
优化方案：
* 第一步：利用shouldComponentUpdate，进行新旧state和props的对比，return false减少不必要的render
* 第二步：优化对比，减少深对比，增加浅对比（对于对象，仅比较引用地址是否相同）。这意味这所有的state都immutable化，每次更新state，都复制原来的state，在新state上进行更改，旧state不动。
 
### (二)Redux的默认性能优化

**【知识点：Redux状态传播路径】**

全局state -> 容器型组件（mapStateToProps函数） -> 展示型组件
简单来说容器型组件与展示型组件是父子关系：
 
| 组件类型      | 数据来源      | 变化通知      | 说明          |
| ------------- | ------------- | ------------- | ------------- |
| 容器型组件    | 全局状态      | 监听全局状态  | 容器型组件一般通过connet函数生成，它订阅了全局状态的变化，通过mapStateToProps函数，我们可以对全局状态进行过滤，只返回该容器型组件关注的局部状态，并将其合并到容器型组件的props上。|
| 展示型组件    | 父组件        | 父组件通知    | 展示型组件不直接从global state获取数据，其数据来源于父组件。|

**【Redux的两个基本假设】**

1. 容器型组件必须为Pure Component，即组件只依赖于state和props
2. 全局状态树(global state)的任何变动都是immutable的

**【Redux的默认性能优化】**

用Redux官方API函数connect生成的容器型组件，默认会提供一个shouldComponentUpdate函数，其中对props和state进行了浅层比较。所以，必须遵循以上两条来更改全局state，要不然容器型组件默认的shouldComponentUpdate函数就是无效的了。

然而对于对每一个展示型组件实现shouldComponentUpdate函数，避免展示型组件多余的虚拟DOM比较。
```javascript
shouldComponentUpdate: function (nextProps, nextState) {
  return !isShallowEqual(this.props,nextProps) || !isShallowEqual(this.state,nextState);
}
```
**【结论】**

对于所有state都是实现immutable化：容器型组件层面，使用Redux的默认性能优化方案；展示型组件层面，使用常规React组件性能优化方案。
 
 
 
参考资料：
* [http://web.jobbole.com/88050/](http://web.jobbole.com/88050/)
* [http://imweb.io/topic/577512fe732b4107576230b9](http://imweb.io/topic/577512fe732b4107576230b9)
* [https://segmentfault.com/a/1190000006254212](https://segmentfault.com/a/1190000006254212)

