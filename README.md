# Februray,2016
## 2016.2.19(待复盘)

> 主要内容：
1.C语言基础
2.C++基础
3.使用github做博客
4.Markdown语法
5.css技巧

### **一、css技巧**

1. 雪碧图
> 原理：使用CSS background和background-position属性渲染

2. 前端图片延迟加载的具体实现
> 原理：先将图片的真实地址缓存在一个自定义的属性(lazy-src)中，而src地址使用一个全透明的占位图片来代替。

3. 让所有浏览器支持元素透明效果
		.trans{
			opacity: .7;
			/* IE8 */
			-ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=70)";
			/* IE5-7 */
			filter: alpha(opacity=70);
		}

4. 巧用 :before和:after
	+ 结合border写个对话框的样式
	
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

	+ 作为内容的半透明背景层
	
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

5. 多行文本溢出显示省略号(…)全攻略
	+ WebKit浏览器或移动端的页面

			overflow : hidden;
			text-overflow: ellipsis;
			display: -webkit-box;
			-webkit-line-clamp: 2;
			-webkit-box-orient: vertical;

	+ 跨浏览器兼容的方案

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


### **二、快速排序js实现**

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

### **三、Markdown语法**
>Markdown 官方文档
[创始人 John Gruber 的 Markdown 语法说明](http://daringfireball.net/projects/markdown/syntax)
[Markdown 中文版语法说明](http://wowubuntu.com/markdown/#list)

1. 标题设置（让字体变大，和word的标题意思一样）
在Markdown当中设置标题，有两种方式：
第一种：通过在文字下方添加“=”和“-”，他们分别表示一级标题和二级标题。
第二种：在文字开头加上 “#”，通过“#”数量表示几级标题。（一共只有1~6级标题，1级标题字体最大）

2. 块注释（blockquote）
通过在文字开头添加“>”表示块注释。（当>和文字之间添加五个blank时，块注释的文字会有变化。）

3. 斜体
将需要设置为斜体的文字两端使用1个“*”或者“_”夹起来

4. 粗体
将需要设置为斜体的文字两端使用2个“*”或者“_”夹起来

5. 无序列表
在文字开头添加(*, +, and -)实现无序列表。但是要注意在(*, +, and -)和文字之间需要添加空格。（建议：一个文档中只是用一种无序列表的表示方式）

6. 有序列表
使用数字后面跟上句号。（还要有空格）

7. 链接（Links）
Markdown中有两种方式，实现链接，分别为内联方式和引用方式。
内联方式：This is an [example link](http://example.com/).
引用方式：
I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].  

[1]: http://google.com/        "Google" 
[2]: http://search.yahoo.com/  "Yahoo Search" 
[3]: http://search.msn.com/    "MSN Search"
 

8. 图片（Images）
图片的处理方式和链接的处理方式，非常的类似。
内联方式：![alt text](/path/to/img.jpg "Title")
引用方式：
![alt text][id] 

[id]: /path/to/img.jpg "Title"

9. 代码（HTML中所谓的Code）
实现方式有两种：
第一种：简单文字出现一个代码框。使用`<blockquote>`。（`不是单引号而是左上角的ESC下面~中的`）
第二种：大片文字需要实现代码框。使用Tab和四个空格。

10. 脚注（footnote）
实现方式如下：
hello[^hello]


[^hello]: hi

11. 下划线
在空白行下方添加三条“-”横线。（前面讲过在文字下方添加“-”，实现的2级标题）

---

## 2016.2.18

> 主要内容：   
1. Git([廖雪峰Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000))
2. ajax  
3. PS切图
4. [JavaScript秘密花园](https://bonsaiden.github.io/JavaScript-Garden/zh/)

---

## 2016.2.17

> 主要内容：   
1. 正则表达式  
2. 页面布局  
3. jquery-ui
4. jquery-mobile

### **一、正则表达式**
1. 使用
	>/正则表达式/ 或 new RegExp("正则表达式")

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
+ 水平居中：行内元素解决方案
>只需要把行内元素包裹在一个属性display为block的父层元素中，并且把父层元素添加如下属性即可：

		.parent {
		    text-align:center;
		}
	
+ 水平居中：块状元素解决方案

		.item {
		    /* 这里可以设置顶端外边距 */
		    margin: 10px auto;
		}
	
+ 水平居中：多个块状元素解决方案
> 将元素的display属性设置为inline-block，并且把父元素的text-align属性设置为center即可:

			.parent {
			    text-align:center;
			}
			
+ 水平居中：多个块状元素解决方案 (使用flexbox布局实现)
> 使用flexbox布局，只需要把待处理的块状元素的父元素添加属性display:flex及justify-content:center即可:

		.parent {
		    display:flex;
		    justify-content:center;
		}

+ 垂直居中：单行的行内元素解决方案

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
+ 垂直居中：多行的行内元素解决方案
> 组合使用display:table-cell和vertical-align:middle属性来定义需要居中的元素的父容器元素生成效果，如下：

		.parent {
		    background: #222;
		    width: 300px;
		    height: 300px;
		    /* 以下属性垂直居中 */
		    display: table-cell;
		    vertical-align:middle;
		}
		
+ 垂直居中：已知高度的块状元素解决方案

		.item{
		    top: 50%;
		    margin-top: -50px;  /* margin-top值为自身高度的一半 */
		    position: absolute;
		    padding:0;
		}

+ 垂直居中：未知高度的块状元素解决方案

		.item{
		    top: 50%;
		    position: absolute;
		    transform: translateY(-50%);  /* 使用css3的transform来实现 */
		}
		
+ 水平垂直居中：已知高度和宽度的元素解决方案1
这是一种不常见的居中方法，可自适应，比方案2更智能，如下：

		.item{
		    position: absolute;
		    margin:auto;
		    left:0;
		    top:0;
		    right:0;
		    bottom:0;
		}
		
+ 水平垂直居中：已知高度和宽度的元素解决方案2

		.item{
		    position: absolute;
		    top: 50%;
		    left: 50%;
		    margin-top: -75px;  /* 设置margin-left / margin-top 为自身高度的一半 */
		    margin-left: -75px;
		}
		
+ 水平垂直居中：未知高度和宽度元素解决方案

		.item{
		    position: absolute;
		    top: 50%;
		    left: 50%;
		    transform: translate(-50%, -50%);  /* 使用css3的transform来实现 */
		}
		
+ 水平垂直居中：使用flex布局实现

		.parent{
		    display: flex;
		    justify-content:center;
		    align-items: center;
		    /* 注意这里需要设置高度来查看垂直居中效果 */
		    background: #AAA;
		    height: 300px;
		}
		
### **三、弹性布局**

> 可参考[阮一峰的网络日志——Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)

---

## 2016.2.16

> 主要内容：   
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
	>false,""(空字符串)，0和NaN，null，undefined

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
	13. reduce(callback[, initialValue])：归并方法

8. 函数：函数是对象，函数名是指针

9. Math舍入方法
	+ ceil向上舍入
	+ floor向上舍入
	+ round标准舍入