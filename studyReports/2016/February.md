# Februray,2016

## 2016.2.29（待复盘）

> **主要内容：**
	1. 深入学习JavaScript对象


## 2016.2.28（待复盘）

> **主要内容：**
	1. cookie与session
	2. URL 详解与 URL 编码
	3. 几种布局方式（双飞翼、圣杯、flex）
	4. AMD与CMD对比
	5. MVC与MVVM


## 2016.2.27（待复盘）

> **主要内容：**
	1. 超文本传输协议（HTTP）介绍
	2. HTTPS和HTTP的区别


## 2016.2.26（待复盘）

> **主要内容：**
	1. 移动端事件
	2. 屏幕尺寸，分辨率，像素，PPI


## 2016.2.25（待复盘）

> **主要内容：**
	1. CSS代码重构与优化
	2. 巧用margin/padding的百分比值实现高度自适应
	3. CSS 小技巧


## 2016.2.24（待复盘）

> **主要内容：**
	1. 17 行代码实现的简易 Javascript 字符串模板
	2. 30行代码实现Javascript中的MVC
	3. apply实现数组降维


## 2016.2.23（待复盘）

> **主要内容：**
	1. 前端图片选择
	2. 运算符中的一些小技巧


## 2016.2.22（待复盘）

> **主要内容：**
	1. IE和firefox浏览器的event事件兼容性
	2. 一道常被人轻视的前端JS面试题


## 2016.2.21

> **主要内容：**
	1. js基础知识

### **一、js知识**

1. 严格模式
	> use strict
	> 可以写在整个函数的的里面，也可以写在整个函数的的最上面，也可以在上面加点东西啥的

	* 不能使用with
	* 变量不声明不能赋值（ReferenceError）
	* arguments变为参数的静态副本
	* delete参数、函数名报错
	* delete不可配置的属性也会报错:Object.defineProperty(obj,'a',{configurable:false})
	* 对象字面量属性名重复报错
	* 禁止八进制的字面量
	* eval arguments变为关键字，不能够作为变量或函数名
	* eval独立作用域
	* 一般函数调用时（不是对象的方法调用，也不使用apply/call/bind等修改this）this指向null，而不是全局对象。若使用apply/call，当传入null或undefined时，this将指向null或undefined，而不是全局对象。
	* 试图修改不可写属性(writable=false)，在不可扩展的对象上添加属性时报TypeError，而不是忽略。
	* arguments.caller , arguments.callee被禁用

1. 包装对象
	当基本类型以对象的方式去使用时，JavaScript会转换成对应的包装类型，相当于new一个对象，内容和基本类型的内容一样，然后当操作完成再去访问的时候，这个临时对象会被销毁，然后再访问时候就是undefined。

	number,string,boolean都有对应的包装类型。

1. 类型检测
    * typeof:返回字符串，适合函数（function）对象和基本类型的判断监测,遇到null失效

	    ```
	    typeof 100 "number"//返回字符串number
	    typeof true "boolean"
	    typeof function "function"
	    typeof (undefined) "undefined"
	    typeof new Object() "object"
	    typeof [1,2] "object"//没有经过特殊处理
	    typeof NaN "number"
	    typeof null "object"//兼容问题
	    ```

    * instanceof:适用于自定义对象，也可以监测原生对象
	    obj .. Object//基于原型链
	    左操作数期望是对象，右操作数必须是函数对象，否则会抛出type error
	    会判断左操作数在原型链上是否有右边构造函数的prototype属性
	    任何一个构造函数都有一个prototype属性
	    不同window或iframe间的对象类型检测不能使用instanceof
    
    * Object.prototype.toString:遇到null,undefiend失效
    
        ```
        Object.prototype.toString.apply([])==="[object Array]"//判断数组
        ```
    
    * constructor:任何一个对象都有一个constructor属性，指向构造这个对象的构造器或者构造函数，可以被改写
    * duck type
    	比如不知道这个是不是数组，可以判断他的特征：length数字，是否有joying,push等等

1. 继承
* Student.protptype = Rerson.prototype; // 禁止使用，修改子类时会一并修改父类
* Student.prototype = new Person(); // 不推荐使用，使用Person的构造器创建会带回Person的参数
* Student.prototype = Object.create(Person.prototype); //理想的继承方式
    ES5以下没有Object.create()方法，以下为模拟方式：

    ```
    if (!Object.create) {
        Object.create = function(proto) {
            function F() {}
            F.prototype = proto;
            return new F;
        };
    }
    ```

1. try catch
	> try语句如果抛出异常，则执行catch语句，否则不执行，无论有没有异常，都执行finally语句；try语句必须跟catch或finally语句中至少一个组合使用。

    **try catch语句的嵌套语句执行顺序：**
    * 如果内部嵌套的try语句抛出异常，但内部没有相配套的catch语句，先执行内部的finally语句，然后跳到最近一层的catch语句执行。
    
    * 如果内部嵌套的try语句抛出异常，内部有相配套的catch语句，先执行此语句处理异常，再执行内部的finally语句。不会再在外部处理异常。
    
    * 如果内部嵌套的try语句抛出异常，内部有相配套的catch语句，并且catch语句也抛出异常，如果内部的catch语句有对异常的处理，先执行异常处理语句，然后执行内部的finally语句，最后执行离内部catch语句抛出异常最近一层的catch语句处理异常。



1. 属性标签（defineProperty,seal,freeze）
	* 属性标签: writeable,enumerable,configurable,value,get/set方法。这些可以为属性提供访问权限的控制。
	![属性标签限制](https://github.com/micky-wj/learnings/blob/master/imgs/2-propertylimit.jpg)
	* Object.preventExtensions()让一个对象变的不可扩展，也就是永远不能再添加新的属性。但是要加上Object.seal(obj)才算是真正的限制修改、删除，此时（configurable = false）。
	* Object.seal() 方法可以让一个对象密封，并返回被密封后的对象。密封对象是指那些不能添加新的属性，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性，但可能可以修改已有属性的值的对象。（configurable = false）
	* Object.freeze() 方法可以冻结一个对象。冻结对象是指那些不能添加新的属性，不能修改已有属性的值，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性的对象。也就是说，这个对象永远是不可变的。该方法返回被冻结的对象。（writeable和configurable都为false）
		> 注意：上述提到的方法只是针对某个对象的，比如说冻结一个对象时，并不会影响对象的原型链，如果想对原型链做类似处理的话，可以通过Object.prototypeof（）的方法拿到对象的原型，一层层遍历，然后freeze
	* Object.isExtensible(obj)判断是否可扩展；Object.isSealed(obj)判断是否被隐藏；Object.isFrozen(obj)判断对象当前是否被冻结。

1. this
    
	* 全局的this //this指向浏览器
	* 一般函数的this //this指向浏览器
	* 作为对象方法的函数的this //this指向对象
	* 对象原型链上的this //this指向对象本身
	* get/set方法中的this //指向对象本身
	* 构造器中的this

	    ```
	    function yy(){ this.a = 33;} var xx = new yy()
	    // this会指向空对象，并且空对象的原型指向一样yy();的prototype属性；当没有return或者return基本类型时，会返回this。如果是对象，则返回该对象。
	    ```

	* apply,call
	    a.call(b,xx,xx)中this指向当前的作用域.这里a方法在b作用域中执行this指向b；
	* var g = f.bind({a:"test"}); //this指向bind的参数对象 {a:"test"}

1. 自执行函数
	> !function, +function, (function(){})();
	> 告诉浏览器自动运行这个匿名函数的，因为!+()这些符号的运算符是最高的，所以会先运行它们后面的函数

1. 原型链
    ![原型链](https://github.com/micky-wj/learnings/blob/master/imgs/1-prototype.jpg)
    ![原型链举例函数](https://github.com/micky-wj/learnings/blob/master/imgs/4-protoexample.jpg)
    注：并不是所有的对象的原型链上都有Object.prototype
	```
	//第一种
	var obj2 = Object.create(null);
	obj2.toString(); //undefined
	//第二种
	var bined = abc.bind(null);//bind修改运行时的this
	bined.prototype;//undefined bind没有prototype属性
	```

1. bind方法模拟
    ![bind方法模拟](https://github.com/micky-wj/learnings/blob/master/imgs/3-bind.jpg)

1. 链式调用
    ```
    window.$ = function(id){
        return new _$(id);
    }
    
    function _$(id){
        this.elements = document.getElementById(id);
    }
    
    _$.prototype = {
        constructor:_$,
        hide:function(){
            console.log('hide');
            return this;
        },
        show:function(){
            console.log('show');
            return this;
        },
        getName:function(callback){
            if(callback){
                callback.call(this,this.name);
            }
            return this;
        },
        setName:function(name){
            this.name = name;
            return this;
        }
    }
    
    $('id').setName('xesam').getName(function(name){
        console.log(name);
    }).show().hide().show().hide().show();    
    ```

1. 模块化
	> 模块化函数内部变量不会泄漏到全局作用域。

	* 定义立即执行函数
	moduleA = function(){}();
	* this 方法

	    ```
	    moduleA = new function(){
	        var prop = 1;
	        function func(){}
	        this.func = func;
	        this.prop = prop;
	    }
	    ```

1. 变量对象variable object(VO)
	**填充顺序:**
	* 函数参数（若未传入，初始化参数则为undefined）
	* 函数声明（若发生冲突，会覆盖）
	* 变量声明（初始化变量值为undefined若发生冲突则会忽略）

## 2016.2.20(待复盘)

> **主要内容：**
	1. HTTP请求过程
	2. 浏览器渲染过程
	3. 响应式设计
	4. 跨域请求方法
	5. 重排重绘
	6. 移动端事件
	7. js回调
	8. 图片格式对比

### **一、HTTP请求与浏览器渲染过程**

1. HTTP请求过程简述
2. 跨域请求方法（jsonp原理）
3. 状态码
4. 浏览器渲染过程
5. 重排与重绘

### **二、响应式设计**

1. 相关名词解释
	（1） 计量单位比较：em，rem，px
	（2） viewport
	（3） 分辨率与像素
	（4） PPI

2. viewport

3. 移动端事件
	touchstart touchmove tounchend

### **三、js回调**

### **四、图片格式对比**


## 2016.2.19

> **主要内容：**
	1. C语言基础
	2. C++基础
	3. 使用github做博客
	4. Markdown语法
	5. css技巧

### **一、css技巧**

1. 雪碧图
	> 原理：使用CSS background和background-position属性渲染

2. 前端图片延迟加载的具体实现
	> 原理：先将图片的真实地址缓存在一个自定义的属性(lazy-src)中，而src地址使用一个全透明的占位图片来代替。

3. 让所有浏览器支持元素透明效果
	```
	.trans{
		opacity: .7;
		/* IE8 */
		-ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=70)";
		/* IE5-7 */
		filter: alpha(opacity=70);
	}
	```

4. 巧用 :before和:after
	* 结合border写个对话框的样式

		```
	    <style>
	    .test-div{
	        position: relative;  /*日常相对定位*/
	        width:150px;
	        height: 36px;
	        border:black 1px solid;
	        border-radius:5px;
	        background: rgba(245,245,245,1)
	    }
	    .test-div:before,.test-div:after{
	        content: "";  /*:before和:after必带技能，重要性为满5颗星*/
	        display: block;
	        position: absolute;  /*日常绝对定位*/
	        top:8px;
	        width: 0;
	        height: 0;
	        border:6px transparent solid;
	    }
	    .test-div:before{
	        left:-11px;
	        border-right-color: rgba(245,245,245,1);
	        z-index:1
	    }
	    .test-div:after{
	        left:-12px;
	        border-right-color: rgba(0,0,0,1);
	        z-index: 0
	    }
	    </style>
	    <div class="test-div"></div>
	    ```

	* 作为内容的半透明背景层

		```
	    <style>
          body{
              background: url(img/1.jpg) no-repeat left top /*这里本兽加了个图片背景，用以区分背景的半透明及内容的完全不透明*/
          }
          .test-div{
              position: relative;  /*日常相对定位(重要，下面内容也会介绍)*/
              width:300px;
              height: 120px;
              padding: 20px 10px;
              font-weight: bold;
          }
          .test-div:before{
              position: absolute;  /*日常绝对定位(重要，下面内容也会略带介绍)*/
              content: "";  /*:before和:after必带技能，重要性为满5颗星*/
              top:0;
              left: 0;
              width: 100%;  /*和内容一样的宽度*/
              height: 100%;  /*和内容一样的高度*/
              background: rgba(255,255,255,.5); /*给定背景白色，透明度50%*/
              z-index:-1 /*日常元素堆叠顺序(重要，下面内容也会略带介绍)*/
          }
      	</style>
	    ```

5. 多行文本溢出显示省略号(…)全攻略
	* WebKit浏览器或移动端的页面

		```
		overflow : hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		```

	* 跨浏览器兼容的方案

		```
		p {
		    position:relative;
		    line-height:1.4em;
		    /* 3 times the line-height to show 3 lines */
		    height:4.2em;
		    overflow:hidden;
		}
		p::after {
		    content:"...";
		    font-weight:bold;
		    position:absolute;
		    bottom:0;
		    right:0;
		    padding:0 20px 1px 45px;
		    background:url(http://www.css88.com/wp-content/uploads/2014/09/ellipsis_bg.png) repeat-y;
		}
		```

### **二、快速排序js实现**
```
var quickSort = function(arr) {
　　if (arr.length <= 1) { return arr; }
　　var pivotIndex = Math.floor(arr.length / 2);
　　var pivot = arr.splice(pivotIndex, 1)[0];
　　var left = [];
　　var right = [];
　　for (var i = 0; i < arr.length; i++){
　　　　if (arr[i] < pivot) {
　　　　　　left.push(arr[i]);
　　　　} else {
　　　　　　right.push(arr[i]);
　　　　}
　　}
　　return quickSort(left).concat([pivot], quickSort(right));
};
```

### **三、Markdown语法**

> **Markdown 官方文档：**
> [创始人 John Gruber 的 Markdown 语法说明](http://daringfireball.net/projects/markdown/syntax)
> [Markdown 中文版语法说明](http://wowubuntu.com/markdown/#list)

1. 标题设置（让字体变大，和word的标题意思一样）
	* 通过在文字下方添加`=`和`-`，他们分别表示一级标题和二级标题。
	* 在文字开头加上`#`，通过`#`数量表示几级标题。（一共只有1~6级标题，1级标题字体最大）

2. 块注释（blockquote）
	通过在文字开头添加`>`表示块注释。（当`>`和文字之间添加五个blank时，块注释的文字会有变化。）

3. 斜体
	将需要设置为斜体的文字两端使用1个`*`或者`_`夹起来

4. 粗体
	将需要设置为斜体的文字两端使用2个`*`或者`_`夹起来

5. 无序列表
	在文字开头添加(`*`, `+`, and -)实现无序列表。但是要注意在(`*`, `+`, and `-`)和文字之间需要添加空格。（建议：一个文档中只是用一种无序列表的表示方式）

6. 有序列表
	使用数字后面跟上句号。（还要有空格）

7. 链接（Links）
	* 内联方式：`This is an [example link](http://example.com/)`.
	* 引用方式：

		```
        I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].  
        
        [1]: http://google.com/        "Google" 
        [2]: http://search.yahoo.com/  "Yahoo Search" 
        [3]: http://search.msn.com/    "MSN Search"
		```

8. 图片（Images）
	* 内联方式：`![alt text](/path/to/img.jpg "Title")`
	* 引用方式：

		```
        ![alt text][id] 
        [id]: /path/to/img.jpg "Title"
        ```

9. 代码（HTML中所谓的Code）
	* 简单文字出现一个代码框。使用\`&lt;blockquote\&gt;\`。（\`不是单引号而是左上角的ESC下面~中的\`）
	* 大片文字需要实现代码框。使用Tab和四个空格。

10. 脚注（footnote）
	```
    hello[^hello]
    [^hello]: hi
    ```

11. 下划线
	在空白行下方添加三条`-`横线。（前面讲过在文字下方添加`-`，实现的2级标题）


## 2016.2.18

> **主要内容：**   
	1. Git([廖雪峰Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000))
	2. ajax  
	3. PS切图
	4. [JavaScript秘密花园](https://bonsaiden.github.io/JavaScript-Garden/zh/)


## 2016.2.17

> **主要内容：**   
	1. 正则表达式  
	2. 页面布局  
	3. jquery-ui
	4. jquery-mobile

### **一、正则表达式**

1. 使用
	> /正则表达式/ 或 new RegExp("正则表达式")

2. 基本元素
	* . 任意字符(除换行符)
	* \d 数字(0-9)
	* \D 非\d
	* \w 数字(0-9)or字母a-z(包括大小写)or下划线
	* \W 非\w
	* \s 空格符，TAB，换行符，换页符
	* \S 非\S
	* \t \r \n \v \f tab 回车 换行 垂直制表符 换页符

3. 限制条件
	* [...] 字符范围以内
	* [^...] 字符范围以外
	* ^ 行首（匹配位置必须在行首）//^la
	* $ 行尾 //la$
	* \b 零边界(例:\bla (o lapa)为true,(olapa)为false)
	* \B 非\b

4. 特殊转移符
	> \ \后面的第一个字符会当成普通的文本字符

5. 分组
	* (...) 一个分组
	* \n n是数字，配合()使用,表示第n个分组的内容,\0表示整个表达式
	* (?:...) 有?:的分组表示不记录在\n时会被忽略

6. 重复
	* 贪婪算法（匹配尽可能多次）
		* x* *前字符重复>=0次
		* x+ *前字符重复>0次
	* 非贪婪算法（匹配尽可能少次）
		* x*? 同x*
		* x+? 同x+

7. 3个flag
	* global 匹配所有，不使用匹配到第一个就会停
	* ignoreCase 不区分大小写
	* multiline 按行检索

8. 正则属性和方法
	* /.../.global
	* /.../.ignoreCase
	* /.../.multiline
	* /.../.source //正则内容
	* /.../.exec('字符串'); //返还正则在字符串中匹配到的字符
	* /.../.test('字符串'); //正则在字符串中是否匹配成功
	* /.../.toString(); //返还正则表达式
	* x.compole("y") //将x中的正则替换为y

### **二、CSS居中实现完整攻略**

1. 水平居中：行内元素解决方案
	只需要把行内元素包裹在一个属性display为block的父层元素中，并且把父层元素添加如下属性即可：
	```
	.parent {
	    text-align:center;
	}
	```

1. 水平居中：块状元素解决方案
	```
	.item {
	    /* 这里可以设置顶端外边距 */
	    margin: 10px auto;
	}
	```
	
1. 水平居中：多个块状元素解决方案
	将元素的display属性设置为inline-block，并且把父元素的text-align属性设置为center即可:

	```
	.parent {
	    text-align:center;
	}
	```	

1. 水平居中：多个块状元素解决方案 (使用flexbox布局实现)
	使用flexbox布局，只需要把待处理的块状元素的父元素添加属性display:flex及justify-content:center即可:

	```
	.parent {
	    display:flex;
	    justify-content:center;
	}
	```

1. 垂直居中：单行的行内元素解决方案
	```
	.parent {
	    background: #222;
	    height: 200px;
	}

     /* 以下代码中，将a元素的height和line-height设置的和父元素一样高度即可实现垂直居中 */
	a {
	    height: 200px;
	    line-height:200px; 
	    color: #FFF;
	}
	```

1. 垂直居中：多行的行内元素解决方案
	组合使用display:table-cell和vertical-align:middle属性来定义需要居中的元素的父容器元素生成效果，如下：

	```
	.parent {
	    background: #222;
	    width: 300px;
	    height: 300px;
	    /* 以下属性垂直居中 */
	    display: table-cell;
	    vertical-align:middle;
	}
	```

1. 垂直居中：已知高度的块状元素解决方案
	```
	.item{
	    top: 50%;
	    margin-top: -50px;  /* margin-top值为自身高度的一半 */
	    position: absolute;
	    padding:0;
	}
	```

1. 垂直居中：未知高度的块状元素解决方案
	```
	.item{
	    top: 50%;
	    position: absolute;
	    transform: translateY(-50%);  /* 使用css3的transform来实现 */
	}
	```
		
1. 水平垂直居中：已知高度和宽度的元素解决方案1
	这是一种不常见的居中方法，可自适应，比方案2更智能，如下：
	```
	.item{
	    position: absolute;
	    margin:auto;
	    left:0;
	    top:0;
	    right:0;
	    bottom:0;
	}
	```
		
1. 水平垂直居中：已知高度和宽度的元素解决方案2
	```
	.item{
	    position: absolute;
	    top: 50%;
	    left: 50%;
	    margin-top: -75px;  /* 设置margin-left / margin-top 为自身高度的一半 */
	    margin-left: -75px;
	}
	```

1. 水平垂直居中：未知高度和宽度元素解决方案
	```
	.item{
	    position: absolute;
	    top: 50%;
	    left: 50%;
	    transform: translate(-50%, -50%);  /* 使用css3的transform来实现 */
	}
	```	

1. 水平垂直居中：使用flex布局实现
	```
	.parent{
	    display: flex;
	    justify-content:center;
	    align-items: center;
	    /* 注意这里需要设置高度来查看垂直居中效果 */
	    background: #AAA;
	    height: 300px;
	}
	```

### **三、弹性布局**

> 可参考[阮一峰的网络日志——Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)


## 2016.2.16

> **主要内容：**
	1. html基础  
	2. css基础  
	3. 《JavaScript高级程序设计》1-5章

### **一、html**

1. Html5新特性
	* <strong>语义</strong>：能够让你更恰当地描述你的内容是什么。
	* <strong>连通性</strong>：能够让你和服务器之间通过创新的新技术方法进行通信。
	* <strong>离线 & 存储</strong>：能够让网页在客户端本地存储数据以及更高效地离线运行。
	* <strong>多媒体</strong>：使 video 和 audio 成为了在所有 Web 中的一等公民。
	* <strong>2D/3D 绘图 & 效果</strong>：提供了一个更加分化范围的呈现选择。
	* <strong>性能 & 集成</strong>：提供了非常显著的性能优化和更有效的计算机硬件使用。
	* <strong>设备访问</strong>：能够处理各种输入和输出设备。
	* <strong>样式设计</strong>：让作者们来创作更加复杂的主题吧！

### **二、css**

1. CSS3选择器
	+ 基本选择器
		* 通配选择器 「*」
		* 元素选择器 「Element」
		* ID选择器 「#id」
		* 类选择器 「.class」
		* 群组选择器 「selector1，…，selectorN」
	+ 层次选择器
		* 后代选择器「E F」
		* 子选择器「E > F」
		* 相邻兄弟元素选择器「E + F」
		* 通用兄弟选择器「E ~ F」
	+ 伪类选择器
		* 动态伪类选择器「E:link,E:visited,E:active,E:hover,E:focus」
		* 目标伪类选择器「E:target」
		* 语言伪类选择器「E:lang(language)」
		* 状态伪类选择器「E:checked,E:enabled,E:disabled」
		* 结构伪类选择器「E:first-child,E:last-child,E:root,E:nth-child(n),E:nth-last-child(n),E:nth-of-type(n),E:nth-last-of-type(n),E:first-of-type,E:last-of-type,E:only-child,E:only-of-type,E:empty」
		*  否定伪类选择器「E:not(F)」
	+ 伪元素(对于IE6-8,仅支持单冒号表示方法)
		*  「::first-letter」文本块的第一个字母
		*  「::first-line」元素的第一行文本
		*  「::before,::after」配合使用content属性可以插入额外内容的位置
		*  「::selection」突出显示选中的文本。但是使用前需要确认浏览器对它的支持程度。
	+ 属性选择器
		*  「 E[attr] 」用来选择有某个属性的元素，不论这个属性的值是什么。
		*  「 E[attr=val] 」用来选取具有属性attr并且属性值为val的元素。
		*  「 E[attr|=val] 」用来选择具有属性attr且属性的值为val或以val-开头的元素(其中-是不可或缺的)。
		*  「 E[attr~=val] 」当某个元素的某个属性具有多个用空格隔开的属性值，此时使用E[attr~=val]只要attr属性多个属性值中有一个于val匹配元素就会被选中。
		*  「 E[attr^=val] 」用来选择属性attr的属性值是以val开头的所有元素，注意它与E[attr|=val]的区别，attr|=val中-是必不可少的，也就是说以val-开头。
		*  「 E[attr$=val] 」这个选择器刚好跟E[attr^=val]相反，用来选择具有attr属性且属性值以val结尾的元素。

### **三、Javascript**

1. 数据类型
	+ 简单数据类型：undefined，null，boolean，number，string
	+ 复杂数据类型：object

2. 转换为false的值
	> false,""(空字符串)，0和NaN，null，undefined

3. 结果为NaN的运算
	+ 0/0
	+ Infinity*0
	+ Infinity/Infinity
	+ Infinity%有限大数值（Infinity/非0值为Infinity）
	+ Infinity%Infinity
	+ Infinity+（-Infinity）

4.  运算优先级

	| 运算符           | 说明 |
	|-------------|-----------------|
	|.[ ] ( )| 字段访问、数组索引、函数调用和表达式分组|
	|++ -- - ~ ! delete new typeof void|一元运算符、返回数据类型、对象创建、未定义的值|
	|* / %|相乘、相除、求余数|
	|+ - +|相加、相减、字符串串联|
	|<< >> >>>|移位|
	|< <= > >= instanceof|小于、小于或等于、大于、大于或等于、是否为特定类的实例|
	|== != === !==|相等、不相等、全等，不全等|
	|&|按位“与”|
	|^|按位“异或”|
	|&brvbar;|按位“或”|
	|&&|逻辑“与”|
	|&brvbar;&brvbar;|逻辑“或”|
	|?:|条件运算|
	|= OP=|赋值、赋值运算（如 += 和 &=）|
	|,|多个计算|
	
5. ==和!=操作符遵循的基本规则
	+ 如果有布尔值，将布尔转换为数字。（false是0，true是1）
	+ 如果有NaN，一律返回false。
	+ 如果一个是字符串，另一个是数字，字符串转数字。
	+ 如果一个是对象，则遵循下列规则： 
		+ null, undefined相等，返回true；
		+ 比较之前，不能将null、undefined转换为任何值；
		+ 如果对方不是对象，对象取valueOf进行比较，没有则调用toString方法
		+ 如果对方是对象，判断是不是一个对象；

6. 快速类型转换    
	+ 转换为字符串+’‘      
	+ 转换为数字-0，*1    
	+ 转换为布尔!!

7. 数组方法
	+ shift():删除数组的第一个元素,返回删除的值。
	+ unshift():把参数加载数组的前面，返回数组的长度。
	+ pop():删除数组的最后一个元素，返回删除的值。
	+ push():将参数加载到数组的最后，返回数组的长度。
	+ concat(3,4):把两个数组拼接起来。（不修改原数组）
	+ splice(start,deleteCount,val1,val2,...)：从start位置开始删除deleteCount项，并从该位置起插入val1,val2,... 
	+ reverse：将数组反序 
	+ sort(orderfunction)：按指定的参数对数组进行排序 
	+ slice(start,end)：返回从原数组中指定开始下标到结束下标之间的项组成的新数组（不修改原数组）
	+ join(separator)：将数组的元素组起一个字符串，以separator为分隔符，省略的话则用默认用逗号为分隔符（不修改原数组）
	+ indexOf()和lastIndexOf()返回要查找的元素在数组中的位置（从0开始），不存在则返回-1。
	+ 迭代方法
		+ every(fun)：对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true。
		+filter(fun)：对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。
		+ map(fun)：对数组中的每一项运行给定函数，返回每次调用的结果组成的数组。
		+ some(fun)：对数组中的每一项运行给定函数，如果函数对任一项返回true，则返回true。
		+ forEach(fun)：对数组中的每一项运行给定函数，无返回值。
		+ reduce(callback[, initialValue])：归并方法

8. 函数：函数是对象，函数名是指针

9. Math舍入方法
	+ ceil向上舍入
	+ floor向上舍入
	+ round标准舍入