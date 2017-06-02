## 2016-12-14 webpack之loaders

> 背景：在文件中引入了css文件，但是运行webpack的时候，提示缺少css-loader，因此查看loader功能，主要是加载各种类型文件的引擎。
 
## 一、它是什么鬼？
官网如是说：loaders是你用在app源码上的转换元件。它是用node.js运行的，把源文件作为参数，返回新的资源的函数。
有些晦涩难懂是吧，之后看了某大神的理解一下子豁然开朗。
> webpack其实运行在node下的一个编译站，它可以将各种各样的文件打包起来，包括图片呀、css呀、视频呀，但无论怎么打包最后导出的都是javascrit，但是我们最终被客户端拉出的页面需要css的渲染、需要图片的路径，而loader它可以把各种各样的资源文件进行转变编译，最后用正确的格式加载到浏览器中，比如css被转换为style插入到页面，图片被转换为base64格式。

总而言之，就是把js文件转换成你想要的样子。
 
## 二、有啥特性？
 
* 可以**链式拼接**（使每个loader相对独立）
	举个例子，比如处理less文件，需要如此`'style!css!sass'`配置，意味着先用sassloader处理然后是css-loader，最后是style-loader。
	注：这里提示下，compiler是由右向左执行，第一个 Loader 将会拿到需处理的原内容，上一个 Loader 处理后的结果回传给下一个接着处理，最后的 Loader 将处理后的结果以 String 或 Buffer 的形式返回给 compiler
* 可同步的，也可异步
* **跑在node**里面，也就意味着有很多可能，比如使用node的各种api~
* 能**接受请求参数**，这样就可以传入一些配置给loader
* 在webpack config 中 loaders可以绑定到 扩展名/正则
* 可以通过npm发布或者安装。 
* 除了用package.json 的main入口文件导出的loader外，**一般的模块也可导出一个loader**。 
* 可以访问webpack配置。 
* **插件**可以带给loaders更多功能
* 可以发出出其它**任意格式**的文件
 
## 三、node咋解析loader的？
Loaders 的解析和 modules 类似。一个loader预期导出一个函数，并且被node写为可兼容的js。通常情况你可以用npm管理loader，但是也可以把它当作一个文件在你的app里面引入。又是一段晦涩难懂的话，从下面最简单的loader即可了解一切，其实就是node_module~
```js
// base loader
module.exports = function(source) {
  return source;
};
```
## 四、咋使？
### 方法一：require方式(在想要试用的js文件夹里直接加载)（不建议）
> 提示：如果你希望你的脚本跨平台（nodejs和浏览器端），在可能的情况下避免使用这种方式。可以尝试使用接下来要讲到的config配置方式
 
通过require声明（define，require.ensure,等等）来加载指定的loaders ，使用!来分割资源loaders，每一部分会被解析到当前的文件。不过它可能覆盖config配置里用 "！"规定的全部loader。
```js
require("./loader!./dir/file.txt");
// => 使用 当前目录下"loader.js" 文件转换在"dir"里的"file.txt".

require("jade!./template.jade");
// => 使用 "jade-loader" (安装到 "node_modules"里面的)来转化"template.jade"
//   如果config配置里还有别的loader绑定到该文件，那么那个loader会被也会调用.

require("!style!css!less!bootstrap/less/bootstrap.less");
// => 转化顺序"bootstrap.less" =>"less-loader"=>"less-loader"=>"style-loader"
```
### 方法二：config配置方式（最常用）
可以将loader绑到正则里面
```js
{
    module: {
        loaders: [
            { test: /\.jade$/, loader: "jade" },
            // => "jade" loader is used for ".jade" files
            { test: /\.css$/, loader: "style!css" },
            // => "style" and "css" loader is used for ".css" files
            // Alternative syntax:
            { test: /\.css$/, loaders: ["style", "css"] },
        ]
    }
}
```
### 方法三：命令行方式
通过CLI将loader绑定到一个扩展里面：
```js
$ webpack --module-bind jade --module-bind 'css=style!css'
// 表示 使用 "jade" 转换 ".jade" 文件， 使用 "style" 和 "css" 转换 ".css" 文件
```
## 五、主要语法
1. 用'!'分隔各个loader
2. 像浏览器url那样传参，比如`url-loader?mimetype=image/png`
	> 提示：请求串的格式取决于loader。可以参照[loader的文档]( https://github.com/webpack/docs/wiki/list-of-loaders )。大部分的loader都接受标准格式(`?key=value&key2=value2`)和json格式(`?{"key":"value","key2":"value2"}`)。

## 六、技能提升：咋造？
根据以上总结的loader特性可知，基本得满足以下条件
* 从右到左，链式执行
* 上一个 Loader 的处理结果给下一个接着处理
* 使用node module写法
* 可异步

具体可看这个[教程](http://web.jobbole.com/84851/)

## 附录

[这个网址](https://segmentfault.com/a/1190000005742111)有列举一些比较常用的loader写法
 
以上仅我薄见，如果有不对的地方，望大家指正。
