##2016-9-4 chrome开发者工具介绍

一、Element

1、基本功能：主要是显示实时的DOM树、样式、事件等

2、右边选项卡介绍：

Styles：样式表、按权重排列

Computed：当前元素的盒模型及各种数值

Event Listeners：绑定到当前元素的事件监听函数

DOM Breakpoints：DOM断点（在DOM被修改时，在修改的代码前挂起）

Properties：当前元素作为DOM对象的各个属性

3、技巧：

在DOM树面板，右键点击元素，选择Break on：为该元素添加dom操作事件监听。包含三个选项（树结构改变、属性改变、节点移除） 

在 Event Listeners 选项卡中，在绑定的事件上右键可以跳转到相应的 JS 代码上

点击左上角的第一个图标，然后把鼠标移动到视图界面中，对准元素按下鼠标左键，对应的DOM树面板会定位到选择的元素

使用 Ctrl + F 打开搜索框，除了常规字符串还可以使用选择器来选择 HTML 元素

右键点击内联的 JS 或者 CSS 路径，选择 open 可以在 Sources 面板中打开相应的文件

样式数值调整快捷键

Up / Down，增加或减少 1 单位

Shift + Up / Down，增加或减少 10 单位

Alt + Up / Down，增加或减少 0.1 单位

鼠标滚轮

二、Network

1、基本功能：监控当前网页所有的http请求

2、字段说明

Name：请求文件名称

Method：方法（常见的是get post）

Status：请求完成的状态

Type：请求的类型

Initiator：请求源也就是说该链接通过什么发送（常见的是Parser、Script）

Size：下载文件或者请求占的资源大小

Time：请求或下载的时间

Timeline：该链接在发送过程中的时间状态轴（我们可以把鼠标移动到这些红红绿绿的时间轴上，对应的会有它的详细信息：开始下载时
间，等待加载时间，自身下载耗时）

3、技巧

单击面板中的任意一条http信息，会在底部弹出一个新的面板，其中记录了该条http请求的详细参数header（表头信息、返回信息、请求
基本状态）、Preview（返回的格式化转移后文本信息）、response（转移之前的原始信息）、Cookies（该请求带的cookies）、Timing（请求时间变化）

面板的底部有记录了整体网络请求状态的一些基本信息

filter 输入框输入 Is:running 指令可以查看正在进行中的网络请求

三、Resources

1、基本功能：当前界面所加载的资源列表，以及cookie和local storage 、SESSION 
等本地存储信息。在这里，可以自由地修改、增加、删除本地存储。

四、Sources

1、基本功能：当前界面所加载的资源列表，主要进行js断点调试。

2、区域1

Sources选项卡: 包含该项目的静态资源文件。双击选中文件，该文件内容会在区域2中显示，如果你选中的是js文件，那么你可以在区域2
种单击行号进行断点调试，只要js执行到了你所标记的这一行，它会停止向下执行并且等待你的命令

Content script 选项卡：包含着一些第三方插件或者浏览器自身的js代码，经常是被忽略的。

Snippets选项卡：在空白处右键后选择弹出的new选项，建立一个自己的新文件，然后在区域2中编辑它。

3、区域3

Call Stack：断点处的债堆栈，就是从该函数起，逐级追寻调用到他的函数名

Breakpoints：在区域2的断点调试信息。当某个断点在执行的时候对应的信息会高亮，双击该处信息可以在区域2中快速定位。

Dom Breakpoints：添加的Dom监控信息

XHR Breakpoints：单击+ 并输入 URL 包含的字符串即可监听该 URL 的 Ajax 请求，输入内容就相当于 URL 
的过滤器。如果什么都不填，那么就监听所有 XHR 请求。一旦 XHR 调用触发时就会在request.send() 的地方中断。

Event Listener Breakpoints：为网页添加各种类型的断点信息。如选中了Mouse中的某一项（click），当在网页上出发这个动作（单击
网页任意地方），浏览器就是立刻断点监控该事件。

4、技巧

使用 Ctrl + O 快捷键打开搜索框，搜索框下会提示当前页面的涉及的文件，输入文件名即可打开，如果输入 
:5:9，则表示跳转到文件的第五行第九个字符

使用 Alt + - 和 Alt + = 可以在上一个鼠标位置和下一个鼠标位置之间跳转

在预览图片上右键选择 copy image as Data URI，可以将图片转换为 base64 编码

按住 Alt 键然后选择文件内容，可以创建一个矩形选区

在 JS 文件中选中一段代码，通过 Ctrl + Shift + E 可以在 Console 面板中运行这段代码

使用 Ctrl + Shift + F 可以搜索所有文件的信息

五、Timeline

1、基本功能：描述网站在某些时候做的事情和呈现出的状态，用来分析网页性能。

2、详述：http://web.jobbole.com/82576/

3、技巧

在面板中会有一些帧使用红色突出显示，这是因为这些帧值得引起开发者注意，它们的渲染时间通常超过了 
18ms。点击这些红色的帧，即可查看相应的警告信息。通常认为每秒渲染 60 帧的页面是流畅的，这就要求每一帧的渲染不能超过 16ms。

六、Profiles

1、基本功能：主要是CPU和内存占用的信息。

2、类型：

Collect JavaScript CPU Profile：监控函数执行期花费的时间

Take Heap Snapshot：为当前界面拍一个内存快照

Record Heap Allocations：实时监控记录内存变化(对象分配跟踪)

3、详述：http://web.jobbole.com/82590/

4、技巧：

 使用”3步快照”技术来找出JavaScript内存泄露、

页面执行一个能引起内存泄露的操作；

点击“Take Snapshot”来执行一个堆快照；

重复执行步骤 a 和步骤 b三次；

选择最后一个堆快照；

将过滤器从“所有对象”改为“快照 1 和 2 之间的对象”；

现在应该已经可以看到一组新的泄露对象的集合，选择其中的一个然后查看是什么导致的内存泄露

七、Audits

1、基本功能：对页面的优化建议

八、Console

1、基本功能：用于输出和显示调试信息（）

2、console方法（详情参考：http://www.cnblogs.com/Wayou/p/chrome-console-tips-and-tricks.html）：

console.log(object[, object, ...])：向控制台输出一条消息，支持 C 语言 printf 式的格式化输出。

console.info(object[, object, ...])：向控制台输出一条信息，该信息包含一个表示“信息”的图标，和指向该行代码位置的超链接。

console.error(object[, object, ...])：在控制台中输出一条错误信息。

console.warn(object[, object, ...])：在控制台中输出一条警告信息。

console.assert([, object, ...])：测试一条表达式是否为真，不为真时将抛出异常（断言失败）。

console.dir(object)：输出一个对象的全部属性（输出结果类似于 DOM 面板中的样式）。

console.dirxml(node)： 输出一个 HTML 或者 XML 元素的结构树，点击结构树上面的节点进入到 HTML 面板。

console.trace()：输出 Javascript 执行时的堆栈追踪。

console.group(object[, object, ...])：输出消息的同时打开一个嵌套块，用以缩进输出的内容，调用 console.groupEnd() 
用以结束这个块的输出。

console.time(name)：计时器，当调用 console.timeEnd(name)并传递相同的 name 
为参数时，计时停止，并输出执行两条语句之间代码所消耗的时间（毫秒）。

console.profile([title]) 与 profileEnd()：结合使用，用来做性能测试，与 console 面板上 profile 按钮的功能完全相同。

console.count([title])：输出该行代码被执行的次数，参数 title 将在输出时作为输出结果的前缀使用。

console.clear()：清空控制台。

3、技巧

使用 inspect(elem)，跳转到 elements 面板的指定元素节点

使用 copy(values)，将数据复制到剪贴板

使用 values(object)，获取对象的所有属性值，返回数组

使用 Ctrl + L，清空当前的 console 面板

使用 getEventListeners(node) 函数可以获取当前节点绑定的事件，返回一个数组

使用 console.table(arr) 输出数组数据

