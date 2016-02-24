# Februray,2016
## 2016.2.19(待复盘)

> 主要目标：后台语言和数据机构的学习

### **一、雪碧图**
### **二、前端图片延迟加载的具体实现**
### **三、6个CSS开发的实用小技巧**
### **四、C语言基础**
### **五、C++基础**
### **六、使用github做博客**
### **七、Markdown语法**
### **八、数据结构**
### **九、NodeJS**
### **十、《JavaScript高级程序设计》**

***

## 2016.2.18(待复盘)

> 主要目标：熟悉一些工具的使用

### **一、Git**
### **二、ajax**
### **三、PS切图**
### **四、《JavaScript高级程序设计》6-7章**
### **五、js秘密花园**

***

## 2016.2.17(待复盘)

> 主要目标：布局和正则的学习

### **一、正则表达式**
### **二、页面布局**
#### **1. 居中实现**
#### **2. 弹性布局**
### **三、jquery-ui**
### **四、jquery-mobile**
### **五、《JavaScript高级程序设计》5章**

***

## 2016.2.17

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
	1. 如果有布尔值，将布尔转换为数字。（false是0，true是1）
	2. 如果有NaN，一律返回false。
	3. 如果一个是字符串，另一个是数字，字符串转数字。
	4. 如果一个是对象，则遵循下列规则： 
		+ null, undefined相等，返回true；
		+ 比较之前，不能将null、undefined转换为任何值；
		+ 如果对方不是对象，对象取valueOf进行比较，没有则调用toString方法
		+ 如果对方是对象，判断是不是一个对象；

6. 快速类型转换    
	1.转换为字符串+’‘      
	2.转换为数字-0，*1    
	3.转换为布尔!!
7. 数组方法
	1. shift():删除数组的第一个元素,返回删除的值。
	2. unshift():把参数加载数组的前面，返回数组的长度。
	3. pop():删除数组的最后一个元素，返回删除的值。
	4. push():将参数加载到数组的最后，返回数组的长度。
	5. concat(3,4):把两个数组拼接起来。（不修改原数组）
	6. splice(start,deleteCount,val1,val2,...)：从start位置开始删除deleteCount项，并从该位置起插入val1,val2,... 
	7. reverse：将数组反序 
	8. sort(orderfunction)：按指定的参数对数组进行排序 
	9. slice(start,end)：返回从原数组中指定开始下标到结束下标之间的项组成的新数组（不修改原数组）
	10. join(separator)：将数组的元素组起一个字符串，以separator为分隔符，省略的话则用默认用逗号为分隔符（不修改原数组）
	11.  indexOf()和lastIndexOf()返回要查找的元素在数组中的位置（从0开始），不存在则返回-1。
	12.  迭代方法
		+ every(fun)：对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true。
		+filter(fun)：对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。
		+ map(fun)：对数组中的每一项运行给定函数，返回每次调用的结果组成的数组。
		+ some(fun)：对数组中的每一项运行给定函数，如果函数对任一项返回true，则返回true。
		+ forEach(fun)：对数组中的每一项运行给定函数，无返回值。
	13. reduce(callback[, initialValue])：归并方法
8. 函数：函数是对象，函数名是指针
9. Math舍入方法
	1. ceil向上舍入
	2. floor向上舍入
	3. round标准舍入