# Februray,2016

## 2016.2.29（待复盘）

> **主要内容：**  
	1. 深入学习JavaScript对象  

## 2016.2.28

> **主要内容：**  
	1. cookie与session  
	2. URL详解与URL编码  
	3. 几种布局方式（双飞翼、圣杯、flex）  
	4. AMD与CMD对比  
    
### **一、cookie与session**

**（一）二者的定义**

当你在浏览网站的时候，WEB服务器会先送一小小资料放在你的计算机上，Cookie会帮你在网站上所打的文字或是一些选择，都纪录下来。当下次你再光临同一个网站，WEB服务器会先看看有没有它上次留下的Cookie资料，有的话，就会依据Cookie里的内容来判断使用者，送出特定的网页内容给你。Cookie的使用很普遍，许多有提供个人化服务的网站，都是利用Cookie来辨认使用者，以方便送出使用者量身定做的内容，像是 Web 接口的免费 email 网站，都要用到 Cookie。

具体来说cookie机制采用的是在客户端保持状态的方案，而session机制采用的是在服务器端保持状态的方案。

同时我们也看到，由于采用服务器端保持状态的方案在客户端也需要保存一个标识，所以session机制可能需要借助于cookie机制来达到保存标识的目的，但实际上它还有其他选择。 

1. cookie机制

    正统的cookie分发是通过扩展HTTP协议来实现的，服务器通过在HTTP的响应头中加上一行特殊的指示以提示浏览器按照指示生成相应的cookie。然而纯粹的客户端脚本如JavaScript或者VBScript也可以生成cookie。而cookie的使用是由浏览器按照一定的原则在后台自动发送给服务器的。浏览器检查所有存储的cookie，如果某个cookie所声明的作用范围大于等于将要请求的资源所在的位置，则把该cookie附在请求资源的HTTP请求头上发送给服务器。

    cookie的内容主要包括：名字，值，过期时间，路径和域。路径与域一起构成cookie的作用范围。

    若不设置过期时间，则表示这个cookie的生命期为浏览器会话期间，关闭浏览器窗口，cookie就消失。这种生命期为浏览器会话期的cookie被称为会话cookie。会话cookie一般不存储在硬盘上而是保存在内存里，当然这种行为并不是规范规定的。若设置了过期时间，浏览器就会把cookie保存到硬盘上，关闭后再次打开浏览器，这些cookie仍然有效直到超过设定的过期时间。存储在硬盘上的cookie可以在不同的浏览器进程间共享，比如两个IE窗口。而对于保存在内存里的cookie，不同的浏览器有不同的处理方式。

2. session机制

    session机制是一种服务器端的机制，服务器使用一种类似于散列表的结构（也可能就是使用散列表）来保存信息。

    当程序需要为某个客户端的请求创建一个session时，服务器首先检查这个客户端的请求里是否已包含了一个session标识（称为sessionid），如果已包含则说明以前已经为此客户端创建过session，服务器就按照sessionid把这个session检索出来使用（检索不到，会新建一个），如果客户端请求不包含sessionid，则为此客户端创建一个session并且生成一个与此session相关联的sessionid，sessionid的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这个sessionid将被在本次响应中返回给客户端保存。保存这个sessionid的方式可以采用cookie，这样在交互过程中浏览器可以自动的按照规则把这个标识发送给服务器。一般这个cookie的名字都是类似于SEEESIONID。但cookie可以被人为的禁止，则必须有其他机制以便在cookie被禁止时仍然能够把sessionid传递回服务器。 

    经常被使用的一种技术叫做URL重写，就是把sessionid直接附加在URL路径的后面。还有一种技术叫做表单隐藏字段。就是服务器会自动修改表单，添加一个隐藏字段，以便在表单提交时能够把sessionid传递回服务器。比如： 


    ```
    <form name="testform" action="/xxx"> 
    <input type="hidden" name="jsessionid" value="ByOK3vjFD75aPnrF7C2HmdnV6QZcEbzWoWiBYEnLerjQ99zWpBng!-145788764"> 
    <input type="text"> 
    </form>
    ```
    
    实际上这种技术可以简单的用对action应用URL重写来代替。

**（二）cookie 和session 的区别**

1. cookie数据存放在客户的浏览器上，session数据放在服务器上。

2. cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗。考虑到安全应当使用session。

3. session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能。考虑到减轻服务器性能方面，应当使用COOKIE。

4. 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

5. 所以个人建议：

   **将登陆信息等重要信息存放为SESSION；其他信息如果需要保留，可以放在COOKIE中** 

### **二、URL详解与URL编码**

**（一）URL 与 URI**

* URL：(Uniform/Universal Resource Locator 的缩写，统一资源定位符)。
* URI：(Uniform Resource Identifier 的缩写，统一资源标识符)。
* 关系：URI 属于 URL 更低层次的抽象，一种字符串文本标准。就是说，URI 属于父类，而 URL 属于 URI 的子类。URL 是 URI 的一个子集。二者的区别在于，URI 表示请求服务器的路径，定义这么一个资源。而 URL 同时说明要如何访问这个资源（http://）。

**（二）端口与URL标准格式**

* 端口(Port)：相当于一种数据的传输通道。用于接受某些数据，然后传输给相应的服务，而电脑将这些数据处理后，再将相应的回复通过开启的端口传给对方。
* 端口的作用：因为IP地址与网络服务的关系是一对多的关系。所以实际上因特网上是通过IP地址加上端口号来区分不同的服务的。端口是通过端口号来标记的，端口号只有整数，范围是从0 到65535。
* URL 标准格式：

    ```
    scheme://host[:port#]/path/…/[;url-params][?query-string][#anchor]
    
    scheme //有我们很熟悉的http、https、ftp以及著名的ed2k，迅雷的thunder等。
    host //HTTP服务器的IP地址或者域名
    port# //HTTP服务器的默认端口是80，这种情况下端口号可以省略。如果使用了别的端口，必须指明，例如tomcat的默认端口是8080 http://localhost:8080/
    path //访问资源的路径
    url-params //所带参数
    query-string //发送给http服务器的数据
    anchor //锚点定位
    ```

**（三）利用 `<a>`标签自动解析 url**

> 原理是动态创建一个a标签，利用浏览器的一些原生方法及一些正则（为了健壮性正则还是要的），完美解析 URL，获取我们想要的任意一个部分。

代码如下：

```
// This function creates a new anchor element and uses location
// properties (inherent) to get the desired URL data. Some String
// operations are used (to normalize results across browsers).

function parseURL(url) {
    var a = document.createElement('a');
    a.href = url;
    return {
        source: url,
        protocol: a.protocol.replace(':',''),
        host: a.hostname,
        port: a.port,
        query: a.search,
        params: (function(){
        var ret = {},
        seg = a.search.replace(/^\?/,'').split('&'),
        len = seg.length, i = 0, s;
        for (;i<len;i++) {
            if (!seg[i]) { continue; }
                s = seg[i].split('=');
                ret[s[0]] = s[1];
            }
            return ret;
        })(),
        file: (a.pathname.match(/([^/?#]+)$/i) || [,''])[1],
        hash: a.hash.replace('#',''),
        path: a.pathname.replace(/^([^/])/,'/$1'),
        relative: (a.href.match(/tps?:\/[^/]+(.+)/) || [,''])[1],
        segments: a.pathname.replace(/^\//,'').split('/')
    };
}
```

Usage 使用方法：

```
var myURL = parseURL('http://abc.com:8080/dir/index.html?id=255&m=hello#top');
myURL.file; // = 'index.html'
myURL.hash; // = 'top'
myURL.host; // = 'abc.com'
myURL.query; // = '?id=255&m=hello'
myURL.params; // = Object = { id: 255, m: hello }
myURL.path; // = '/dir/index.html'
myURL.segments; // = Array = ['dir', 'index.html']
myURL.port; // = '8080'
myURL.protocol; // = 'http'
myURL.source; // = 'http://abc.com:8080/dir/index.html?id=255&m=hello#top'
```

利用上述方法，即可解析得到 URL 的任意部分。

**（四）escape 、 encodeURI 、encodeURIComponent**

1. escape()
    
    首先想声明的是，W3C把这个函数废弃了，身为一名前端如果还用这个函数是要打脸的。

    escape只是对字符串进行编码（而其余两种是对URL进行编码），与URL编码无关。编码之后的效果是以 %XX 或者 %uXXXX 这种形式呈现的。它不会对 ASCII字符、数字 以及 @ * / + 进行编码。

    根据 MDN 的说明，escape 应当换用为 encodeURI 或 encodeURIComponent；unescape 应当换用为 decodeURI 或 decodeURIComponent。escape 应该避免使用。举例如下：

    
    ```
    encodeURI('https://www.baidu.com/ a b c')
    // "https://www.baidu.com/%20a%20b%20c"
    encodeURIComponent('https://www.baidu.com/ a b c')
    // "https%3A%2F%2Fwww.baidu.com%2F%20a%20b%20c"
    
    //而 escape 会编码成下面这样，eocode 了冒号却没 encode 斜杠，十分怪异，故废弃之
    escape('https://www.baidu.com/ a b c')
    // "https%3A//www.baidu.com/%20a%20b%20c"
    ```

2. encodeURI()

    encodeURI() 是 Javascript 中真正用来对 URL 编码的函数。它着眼于对整个URL进行编码。


    ```
    encodeURI("http://www.cnblogs.com/season-huang/some other thing");
    //"http://www.cnblogs.com/season-huang/some%20other%20thing";
    ```

    编码后变为上述结果，可以看到空格被编码成了%20，而斜杠 `/` ，冒号 `:` 并没有被编码。

    是的，它用于对整个 URL 直接编码，不会对`ASCII字母 、数字 、 ~ ! @ # $ & * ( ) = : / , ; ? + ‘ `进行编码。

    
    ```
    encodeURI("~!@#$&*()=:/,;?+'")
    // ~!@#$&*()=:/,;?+'
    ```

3. encodeURIComponent()

    嘿，有的时候，我们的 URL 长这样子，请求参数中带了另一个 URL：`var URL = "http://www.a.com?foo=http://www.b.com?t=123&s=456";`直接对它进行 encodeURI显然是不行的。因为 encodeURI 不会对冒号 : 及斜杠 / 进行转义，那么就会出现上述所说的服务器接受到之后解析会有歧义。


    ```
    encodeURI(URL)
    // "http://www.a.com?foo=http://www.b.com?t=123&b=456"
    ```
    
    这个时候，就该用到 `encodeURIComponent()` 。它的作用是对URL中的参数进行编码，记住是对参数，而不是对整个 URL 进行编码。因为它仅仅不对` ASCII字母、数字 ~ ! * ( ) ‘` 进行编码。
    
    错误的用法：


    ```
    var URL = "http://www.a.com?foo=http://www.b.com?t=123&s=456";
    encodeURIComponent(URL);
    // "http%3A%2F%2Fwww.a.com%3Ffoo%3Dhttp%3A%2F%2Fwww.b.com%3Ft%3D123%26s%3D456"
    // 错误的用法，看到第一个 http 的冒号及斜杠也被 encode 了
    ```
    
    正确的用法：encodeURIComponent() 着眼于对单个的参数进行编码：
    
    ```
    var param = "http://www.b.com?t=123&s=456"; // 要被编码的参数
    URL = "http://www.a.com?foo="+encodeURIComponent(param);
    //"http://www.a.com?foo=http%3A%2F%2Fwww.b.com%3Ft%3D123%26s%3D456"
    ```

利用上述的使用`<a>`标签解析 URL 以及根据业务场景配合 encodeURI() 与 encodeURIComponent() 便能够很好的处理 URL 的编码问题。应用场景最常见的一个是手工拼接 URL 的时候，对每对 key-value 用 encodeURIComponent 进行转义，再进行传输。

### **三、flex布局基础**

> 原文链接： http://segmentfault.com/a/1190000004320409

1. Flex布局是什么

    Flex是Flexible Box的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。
    
    ```
    //任何一个容器都可以指定为Flex布局。
    .box{
      display: flex;
    }
    
    //行内元素也可以使用Flex布局。
    .box{
      display: inline-flex;
    }
    
    //Webkit内核的浏览器，必须加上-webkit前缀。
    .box{
      display: -webkit-flex; /* Safari */
      display: flex;
    }
    ```
    
    但是，设为Flex布局以后，子元素的float、clear和vertical-align属性将失效。

2. 基本概念

    采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为flex item，简称"项目"。

    容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。

    项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

3. 容器的属性

    * flex-direction：决定主轴的方向（即项目的排列方向）。
    
        ```
        .box {
          flex-direction: row //主轴为水平方向，起点在左端
                | row-reverse //主轴为水平方向，起点在右端。
                | column //主轴为垂直方向，起点在上沿。
                | column-reverse;//主轴为垂直方向，起点在下沿。
        }
        ```
    
    * flex-wrap：默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。
        
        ```
        .box{
          flex-wrap: nowrap | wrap | wrap-reverse;
        }
        // nowrap（默认）：不换行。
        // wrap：换行，第一行在上方。
        // wrap-reverse：换行，第一行在下方。
        ```
    
    * flex-flow：是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap
    
    * justify-content：定义了项目在主轴上的对齐方式。
    
        ```
        .box {
          justify-content: flex-start //（默认值）左对齐
                | flex-end //右对齐
                | center //居中
                | space-between //两端对齐，项目之间的间隔都相等
                | space-around;//每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
        }
        ```
    * align-items：定义项目在交叉轴上如何对齐
    
        ```
        .box {
          align-items: flex-start //交叉轴的起点对齐
            | flex-end //交叉轴的终点对齐
            | center //交叉轴的中点对齐
            | baseline //项目的第一行文字的基线对齐
            | stretch;//默认值，如果项目未设置高度或设为auto，将占满整个容器的高度。
        }
        ```
        
    * align-content：定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
    
        ```
        .box {
          align-content: flex-start | flex-end | center 
            //与交叉轴的起点对齐|终点|中点
            | space-between //与交叉轴两端对齐，轴线之间的间隔平均分布
            | space-around //每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
            | stretch;//轴线占满整个交叉轴
        }
        ```

4. item属性设置

    * order：定义项目的排列顺序。数值越小，排列越靠前，默认为0。

        ```
        .item {
          order: <integer>;
        }
        ```

    * flex-grow：定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

        ```
        .item {
          flex-grow: <number>; /* default 0 */
        }
        ```

    * flex-shrink：定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

        ```
        .item {
          flex-shrink: <number>; /* default 1 */
        }
        ```

        如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。负值对该属性无效。

    * flex-basis：定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
        
        ```
        .item {
          flex-basis: <length> | auto; /* default auto */
        }
        ```
        
        它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。

    * flex：是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。

        ```
        .item {
          flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
        }
        ```
        
        该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

    * align-self：允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

        ```
        .item {
          align-self: auto | flex-start | flex-end | center | baseline | stretch;
        }
        ```
        
        该属性可能取6个值，除了auto，其他都与align-items属性完全一致。

### **四、圣杯布局**

> 原文链接： http://www.cnblogs.com/lyzg/p/5160570.html

### **五、双飞翼布局**

> 原文链接： http://www.imooc.com/wenda/detail/254035

### **五、 AMD规范与CMD规范介绍**

> 原文链接： http://m.blog.chinaunix.net/uid-26672038-id-4112229.html

## 2016.2.27

> **主要内容：**  
	1. 超文本传输协议（HTTP）介绍  
	2. HTTPS和HTTP的区别  

### **一、超文本传输协议（HTTP）介绍**

> 超文本传输协议（HyperText Transfer Protocol，HTTP）是从服务器传输数据到客户端的传输协议。

**（一）HTTP 的主要特点**

1. 支持客户/服务器模式。

2. **简单快速：** 客户向服务器请求服务时，只需传送请求方法和路径。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。

3. **灵活：** HTTP允许传输任意类型的数据对象。传输的类型由 Content-Type 加以标记。

4. **无连接：** 无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。

5. **无状态：** HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

**（二）客户端和服务器端交互的过程**

1. 客户发起连接

2. 客户发送请求

3. 服务器响应请求

4. 服务器关闭连接

**（三）请求消息结构**
> 一个请求消息是由请求行、请求头字段、一个空行和消息主体构成。

```
GET /hello.htm HTTP/1.1
User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)
Host: example.com
Accept-Language: en-us
Accept-Encoding: gzip, deflate
```

1. 请求行
    > 请求行请求消息的第一行就是请求行。它指明使用的请求方法、资源标示符和 HTTP 版本。如 `GET /hello.htm HTTP/1.1`

    **（1）请求方法**

    请求方法用来定义操作资源的方式，HTTP/1.1 协议中定义了八种请求方法：
    
    * GET：读取资源数据
    * POST：新建资源数据
    * PUT：更新资源数据
    * DELETE：删除资源数据
    * HEAD：读取资源的元数据
    * OPTIONS：读取该资源所支持的所有请求方法
    * TRACE：回显服务器收到的请求，主要用于测试或诊断
    * CONNECT：HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。通常用于SSL加密服务器的链接（经由非加密的HTTP代理服务器）
    此外，除了上述方法，特定的HTTP服务器还能够扩展自定义的方法。
    
    **（2）资源标示符**

    资源标示符URI、URL和URN是用来识别、定位和命名互联网上的资源。

    因为要通过多样的方式识别资源（人的名字可能相同，然而计算机文件只能通过唯一的路径名称组合访问），所以需要标准的识别WWW资源的途径。为了满足这种需要，Tim Berners-Lee 引入了标准的识别、定位和命名的途径：URI、URL和URN。
    
    * URI：Uniform Resource Identifier，统一资源标识符
    * URL：Uniform Resource Locator，统一资源定位符
    * URN：Uniform Resource Name，统一资源名称
    
    URL 和 URN 都属于 URI。

    URI 和 URL 的区别是：URL 更具体。URI 和 URL 都定义了什么是资源。但 URL 还定义了如何获得资源。

 
2. 请求头字段
    > 请求头字段用来传递客户端的更多信息，以及传递解析消息主体的必要信息。

	* Accept: 客户端接受哪些 Mine 类型。如 Accept: text/html
	* Accept-Encoding: 支持的编码类型。如 gzip, deflate, sdch
	* Accept-Language: 可接受的语言。如 en-US,en;q=0.8
	* User-Agent:一个标识客户端的字符串。如 User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_5) AppleWebKit/537.36 (KHTML，like Gecko) Chrome/38.0.2125.101 Safari/537.36
	* Cookie: Cookie。如 sessionid=c8422b97-98e2-4bc6-aa31-9b667d6ca4a5; theme=4;
	* Referer: 从哪个页面到的该页面。

3. 空行
    > 空行指示头字段区完成，消息主体开始（如果有消息主体的话）。
4. 消息主体
    > 消息主体是请求消息的承载数据。比如在提交POST表单，并且表单方法不是GET时，表单数据就是打包在消息主体内的。消息主体是可选的。

**（四）响应消息结构**

> 响应消息由一个状态行、响应头字段、一个空行、消息主体构成。

```
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache/2.2.14 (Win32)
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
Content-Length: 88
Content-Type: text/html
Connection: Closed

<html>
   <body>

   <h1>Hello, World!</h1>

   </body>
</html>
```

1. 状态行
    > 状态行由http版本、状态码、状态描述文字构成。如`HTTP/1.1 200 OK`

    **所有的状态码的第一个数字代表了响应的五种状态之一:**
	* 1xx：代表请求已被接受，需要继续处理。这类响应是临时响应，只包含状态行和某些可选的响应头信息，并以空行 结束。
	* 2xx：代表请求接收、理解并且接受。
	* 3xx：代表需要客户端采取进一步的操作才能完成请求。通常，这些状态码用来重定向，后续的请求地址（重定向目 标）在本次响应的Location域中指明。当且仅当后续的请求所使用的方法是GET或者HEAD时，用户浏览器才可以 在没有用户介入的情况下自动提交所需要的后续请求。
	* 4xx：代表了客户端看起来可能发生了错误，妨碍了服务器的处理。除非响应的是一个HEAD请求，否则服务器就应 该返回一个解释当前错误状况的实体，以及这是临时的还是永久性的状况。
	* 5xx：代表了服务器在处理请求的过程中有错误或者异常状态发生，，也有可能是服务器意识到以当前的软硬件资源 无法完成对请求的处理。

    **常见状态码有：**
	* 200: 请求已经成功，请求所希望的响应头或者数据体将随着此响应返回
	* 202: 服务器已接受请求，但尚未处理。正如它可能被拒绝一样，最终该请求可能会也可能不会被执行。在异步操作的场合下，没有比发送这个状态码更方便的做法了
	* 204: 服务器成功处理了请求，但不需要返回任何实体内容，并且希望返回更新了的元信息
	* 304: 被请求的资源内容没有发生更改
	* 400: 包含语法错误，无法被服务器解析
	* 403: 服务器已经接收请求，但是拒绝执行
	* 404: 请求失败，请求所希望得到的资源未在服务器上发现
	* 408: 请求超时。客户端可以再次提交这一请求而无需任何修改
	* 500: 服务器内部错误，无法处理请求
	* 502: 作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效响应
	* 504: 作为网关或者代理工作的服务器尝试执行请求时，未能及时从上游服务器（URI标识出的服务器，例如HTTP、FTP、LDAP）或者辅助服务器（例如DNS）收到响应

2. 响应头字段
    > 和请求消息类似，首部字段会包括服务器本身的一些信息指示、以及响应消息本身的元数据。

    **常见响应头有：**
	* Content-Encoding: 数据的编码类型。如 Content-Encoding: gzip
	* Server: 服务器的名称。如 Server:thin 1.5.0 codename Knife
	* Location: 通知客户端新的资源位置。如 Location: http://www.github.com/login
	* Content-Type: 响应数据的类型。如 Content-Type:text/html; charset=UTF-8
	* Content-Encoding: 响应数据的编码格式。如 gzip。客户端会根据该值对响应内容解码。

3. 消息主体
    > 消息主体是响应消息的承载数据。

### **二、HTTPS和HTTP的区别**

**（一）为什么需要 https**

HTTP 是明文传输的，也就意味着，介于发送端、接收端中间的任意节点都可以知道你们传输的内容是什么。这些节点可能是路由器、代理等。

举个最常见的例子，用户登陆。用户输入账号，密码，**采用 HTTP 的话，只要在代理服务器上做点手脚就可以拿到你的密码了**。

> 用户登陆 → 代理服务器（做手脚）→ 实际授权服务器

在发送端对密码进行加密？没用的，虽然别人不知道你原始密码是多少，但能够拿到加密后的账号密码，照样能登陆。

**（二）HTTPS 是如何保障安全的**

HTTPS 其实就是 **secure http** 的意思啦，也就是 HTTP 的安全升级版。稍微了解网络基础的同学都知道，HTTP 是应用层协议，位于 HTTP 协议之下是传输协议 TCP。TCP 负责传输，HTTP 则定义了数据如何进行包装。
> HTTP → TCP （明文传输）

HTTPS 相对于 HTTP 有哪些不同呢？其实就是在 HTTP 跟 TCP 中间加多了一层加密层 **TLS/SSL**。

1. 神马是 TLS/SSL？

    通俗的讲，TLS、SSL 其实是类似的东西，SSL 是个加密套件，负责对 HTTP 的数据进行加密。TLS 是 SSL 的升级版。现在提到 HTTPS，加密套件基本指的是 TLS。

2. 传输加密的流程

    原先是应用层将数据直接给到 TCP 进行传输，现在改成应用层将数据给到TLS/SSL，将数据加密后，再给到 TCP 进行传输。

**（三）HTTPS 是如何加密数据的**

对安全或密码学基础有了解的同学，应该知道常见的加密手段。一般来说，加密分为对称加密、非对称加密（也叫公开密钥加密）。

1. 对称加密

    对称加密的意思就是，加密数据用的密钥，跟解密数据用的密钥是一样的。

    对称加密的优点在于加密、解密效率通常比较高。缺点在于，数据发送方、数据接收方需要协商、共享同一把密钥，并确保密钥不泄露给其他人。此外，对于多个有数据交换需求的个体，两两之间需要分配并维护一把密钥，这个带来的成本基本是不可接受的。

2. 非对称加密

    非对称加密的意思就是，加密数据用的密钥（公钥），跟解密数据用的密钥（私钥）是不一样的。什么叫做公钥呢？其实就是字面上的意思——公开的密钥，谁都可以查到。因此非对称加密也叫做公开密钥加密。相对应的，私钥就是非公开的密钥，一般是由网站的管理员持有。

    公钥、私钥两个有什么联系呢？

    简单的说就是，通过公钥加密的数据，只能通过私钥解开。通过私钥加密的数据，只能通过公钥解开。

    **很多同学都知道用私钥能解开公钥加密的数据，但忽略了一点，私钥加密的数据，同样可以用公钥解密出来。**而这点对于理解 HTTPS 的整套加密、授权体系非常关键。

3. 举个非对称加密的例子

	* 登陆用户：小明
	* 授权网站：某知名社交网站（以下简称 XX）
	* 步骤一： 小明输入账号密码 → 浏览器用公钥加密 → 请求发送给 XX
	* 步骤二： XX 用私钥解密，验证通过 → 获取小明社交数据，用私钥加密 → 浏览器用公钥解密数据，并展示。

**（四）公开密钥加密：两个明显的问题**

前面举了小明登陆社交网站 XX 的例子，但是单纯使用公开密钥加密存在两个比较明显的问题。

* **问题一：公钥如何获取**

    浏览器是怎么获得 XX 的公钥的？当然，小明可以自己去网上查，XX也可以将公钥贴在自己的主页。然而，对于一个动不动就成败上千万的社交网站来说，会给用户造成极大的不便利，毕竟大部分用户都不知道“公钥”是什么东西。

* **问题二：数据传输仅单向安全**

前面提到，公钥加密的数据，只有私钥能解开，于是小明的账号、密码是安全了，半路不怕被拦截。然后有个很大的问题：私钥加密的数据，公钥也能解开。加上公钥是公开的，小明的隐私数据相当于在网上换了种方式裸奔。（中间代理服务器拿到了公钥后，毫不犹豫的就可以解密小明的数据）

下面就分别针对这两个问题进行解答。

1. 问题一：公钥如何获取

    这里要涉及两个非常重要的概念：证书、CA（证书颁发机构）。

	* 证书

        可以暂时把它理解为网站的身份证。这个身份证里包含了很多信息，其中就包含了上面提到的公钥。

        也就是说，当小明、小王、小光等用户访问 XX 的时候，再也不用满世界的找 XX 的公钥了。当他们访问 XX 的时候，XX 就会把证书发给浏览器，告诉他们说，乖，用这个里面的公钥加密数据。

        这里有个问题，所谓的“证书”是哪来的？这就是下面要提到的CA负责的活了。

	* CA（证书颁发机构）

        强调两点：

        * 可以颁发证书的 CA 有很多（国内外都有）。
        * 只有少数 CA 被认为是权威、公正的，这些 CA 颁发的证书，浏览器才认为是信得过的。比如 VeriSign。（CA自己伪造证书的事情也不是没发生过。。。）

    证书颁发的细节这里先不展开，可以先简单理解为，网站向CA提交了申请，CA审核通过后，将证书颁发给网站，用户访问网站的时候，网站将证书给到用户。

2. 问题二：数据传输仅单向安全

    上面提到，通过私钥加密的数据，可以用公钥解密还原。那么，这是不是就意味着，网站传给用户的数据是不安全的？
    答案是：是！！！（三个叹号表示强调的三次方）

    看到这里，可能你心里会有这样想：用了 HTTPS，数据还是裸奔，这么不靠谱，还不如直接用 HTTP 来的省事。

    但是，为什么业界对网站 HTTPS 化的呼声越来越高呢？这明显跟我们的感性认识相违背啊。

    因为：**HTTPS 虽然用到了公开密钥加密，但同时也结合了其他手段，如对称加密，来确保授权、加密传输的效率、安全性。**

    概括来说，整个简化的加密通信的流程就是：

	* 小明访问 XX，XX 将自己的证书给到小明（其实是给到浏览器，小明不会有感知）
	* 浏览器从证书中拿到 XX 的公钥 A
	* 浏览器生成一个只有自己自己的对称密钥B，用公钥 A 加密，并传给 XX（其实是有协商的过程，这里为了便于理解先简化）
	* XX 通过私钥解密，拿到对称密钥 B
	* 浏览器、XX 之后的数据通信，都用密钥 B 进行加密

    注意：对于每个访问 XX 的用户，生成的对称密钥B理论上来说都是不一样的。比如小明、小王、小光，可能生成的就是 B1、B2、B3.
参考下图：

**（五）HTTPS 握手流程**

HTTPS 的数据传输流程整体上跟 HTTP 是类似的，同样包含两个阶段：握手、数据传输。

1. 握手：证书下发，密钥协商（这个阶段都是明文的）

2. 数据传输：这个阶段才是加密的，用的就是握手阶段协商出来的对称密钥


## 2016.2.26

> **主要内容：**  
	1. 移动端事件  
	2. 深入了解viewport和px  

### **一、移动端事件**

**（一）click的300ms的延迟响应**

Click事件在移动手机开发中有300ms的延迟，因为在手机早期，浏览器系统有放大和缩放功能，用户在屏幕上点击两次之后，系统会触发放大或者缩放功能，因此系统做了一个处理，当触摸一次后，在300ms这段时间内有没有触摸第二次，如果触摸了第二次的话，说明是触发放大或缩放功能，否则的话是click事件。因此当click时候，所有用户必须等待于300ms后才会触发click事件。所以当在移动端使用click事件的时候，会感觉到有300ms的迟钝。

**（二）touch触摸事件**

1. 触摸事件介绍
	* touchstart：当手指放在屏幕上触发;
	* touchmove：当手指在屏幕上滑动时，连续地触发;
	* touchend：当手指从屏幕上离开时触发;
	* touchcancel： 当系统停止跟踪时触发; 该事件暂时使用不到;
    由于触摸会导致屏幕动来动去，所以我们可以在这些事件中函数内部使用 event.preventDefault()来阻止掉默认事件(默认滚动).

2. 触摸属性介绍
    * touches: 表示当前跟踪的触摸操作的touch对象的数组。
        > 当一个手指在触屏上时，event.touches.length = 1;当二个手指在触屏上时，event.touches.length=2, 以此类推。

    * targetTouches:特定于事件目标的touch对象的数组。
        > touch事件会冒泡，所以我们可以使用这个属性指出目标对象。

    * changedTouches:表示上次触摸以来发生了什么改变的touch对象的数组。
    
    * 每个touch对象都包含了以下属性：
        * clientX: 触摸目标在视口中的X坐标。
        * clientY: 触摸目标在视口中的Y坐标。
        * Identifier: 标示触摸的唯一ID。
        * pageX: 触摸目标在页面中的X坐标。
        * pageY: 触摸目标在页面中的Y坐标。
        * screenX: 触摸目标在屏幕中的X坐标。
        * screenY: 触摸目标在屏幕中的Y坐标。
        * target: 触摸的DOM节点目标。

**（三）Gestures手势事件**

> 这个事件针对IOS设备上的，一个Gestures事件在两个或更多手指触摸屏幕时触发。

1. 手势事件介绍
    * Gesturestart：当一个手指已经按在屏幕上，而另一个手指又触摸在屏幕时触发。
    * Gesturechange：当触摸屏幕的任何一个手指的位置发生改变的时候触发。
    * Gestureend：当任何一个手指从屏幕上面移开时触发。

    > **触摸事件和手势事件的之间关系：**  
    > 当一个手指放在屏幕上时，会触发touchstart事件，而另一个手指触摸在屏幕上时触发gesturestart事件，随后触发基于该手指的touchstart事件。  
    > 如果一个或两个手指在屏幕上滑动时，将会触发gesturechange事件，但是只要有一个手指移开时候，则会触发gestureend事件，紧接着会触发touchend事件。  

2. 手势的专有属性：

    * rotation: 表示手指变化引起的旋转角度，负值表示逆时针，正值表示顺时针，从0开始；
    * scale: 表示2个手指之间的距离情况，向内收缩会缩短距离，这个值从1开始的，并随距离拉大而增长。


**（四）基本知识点**

1. 判断是否为iPhone

    ```
    function isAppleMobile() {
        return (navigator.platform.indexOf('iPad') != -1);
    };
    ```

2. 自动大写与自动修正

    要关闭这两项功能，可以通过autocapitalize 与autocorrect 这两个选项：

    ```
    <input type="text" autocapitalize="off" autocorrect="off"/>`
    ```

3. 禁止iOS弹出各种操作窗口

    ```
    -webkit-touch-callout:none
    ```

4. 禁止用户选中文字

    ```
    -webkit-user-select:none
    ```

5. 关于 iOS 系统中，中文输入法输入英文时，字母之间可能会出现一个六分之一空格

    ```
    this.value = this.value.replace(/u2006/g, '');
    ```

6. Andriod 上去掉语音输入按钮

    ```
    input::-webkit-input-speech-button {display: none}
    ```

7. 判断是否为微信浏览器；

    ```
    function is_weixn(){
        var ua = navigator.userAgent.toLowerCase();
        if(ua.match(/MicroMessenger/i)=="micromessenger") {
            return true;
        } else {
            return false;
        }
    }
    ```

8. 屏幕旋转事件(onorientationchange)

    ```
    //判断屏幕是否旋转的JS代码
    function orientationChange() {
        switch(window.orientation) {
            case 0:
            alert("肖像模式 0,screen-width: " + screen.width + "; screen-height:" + screen.height);
            break;
            case -90:
            alert("左旋 -90,screen-width: " + screen.width + "; screen-height:" + screen.height);
            break;
            case 90:
            alert("右旋 90,screen-width: " + screen.width + "; screen-height:" + screen.height);
            break;
            case 180:
            alert("风景模式 180,screen-width: " + screen.width + "; screen-height:" + screen.height);
            break;
        };
    };
    // 添加测试监听函数代码如下：
    addEventListener('load', function(){
        orientationChange();
    window.onorientationchange = orientationChange;
    });
    ```

### **二、深入了解viewport和px**

> 网址链接：[腾讯游戏@七宝童鞋分享的《深入了解viewport与px》](http://tgideas.qq.com/webplat/info/news_version3/804/7104/7106/m5723/201509/376281.shtml)

1. 什么是绝对长度单位？什么是相对长度单位？
    * **绝对长度单位：** in（inch英寸）、cm（厘米）、mm（毫米）、pt（磅）、pc（pica）。in、cm、mm和实际中的常用单位完全相同。pt是标准印刷上常用的单位，72pt的长度为1英寸。pc也是印刷上用的单位，1pc的长度为12磅。绝对长度单位，虽然理解起来很容易，但是在网页的设计中很少用到。所以我们就忽略它们吧。
    * **相对长度单位：** 是网页设计中使用最多的长度单位，包括px、em、rem等。
 
2. 什么是屏幕尺寸、屏幕分辨率、屏幕像素密度？ 
    * **屏幕尺寸：** 指屏幕的对角线的长度，单位是英寸，1英寸=2.54厘米。
    * **屏幕分辨率：** 指在横纵向上的像素点数，单位是px，1px=1个像素点。一般以纵向像素\*横向像素来表示一个手机的分辨率，如1960\*1080。（这里的1像素值得是物理设备的1个像素点）
    * **屏幕像素密度：** 屏幕上每英寸可以显示的像素点的数量，单位是ppi，即“pixels per inch”的缩写。屏幕像素密度与屏幕尺寸和屏幕分辨率有关，在单一变化条件下，屏幕尺寸越小、分辨率越高，像素密度越大，反之越小。

    > 计算像素密度的公式：  
    > 勾股定理算出对角线的分辨率：√(1920²+1080²)≈2203px  
    > 对角线分辨率除以屏幕尺寸：2203/5≈440dpi  
 
3. 什么是ppi、dpi、dp、dip、sp、px？
    * **ppi：** pixels per inch，屏幕上每英寸可以显示的像素点的数量，即屏幕像素密度。
    * **dpi：** dots per inch，最初用于衡量打印物上每英寸的点数密度，就是打印机可以在一英寸内打多少个点。当dpi的概念用在计算机屏幕上时，就称之为ppi。ppi和dpi是同一个概念，Android比较喜欢使用dpi，IOS比较喜欢使用ppi。
    * **dp、dip：** dp和dip都是Density Independent Pixels的缩写，密度独立像素，可以想象成是一个物理尺寸，使同样的设置在不同手机上显示的效果看起来是一样的。Android和IOS都会通过转换系数让控件适应屏幕的尺寸。一个按钮给了44*44dp的大小，在160dpi密度的时候，按钮就是44*44px大小；在320dpi密度的时候，按钮就是88*88px的大小。不需要我们去书写多套尺寸。
    * **sp：** scale independent pixels，用法与dp类似，是专门用来定义文字大小的，受用户android设备字体设置的影响。
    * **px：** 就是通常所说的像素，是网页设计中使用最多的长度单位。将显示器分成非常细小的方格，每个方格就是一个像素。（网页重构中使用的px和屏幕分辨率的px不一定是一样的大小。）
 
4. 什么是mdpi、hdpi、xdpi、xxdpi？

    Google官方指定按照下列标准区分不同设备的dpi：
    | 名称          | 像素密度范围  |
    |:-------------:|:-------------:|
    | mdpi          | 120dpi-160dpi |
    | hdpi          | 160dpi-240dpi |
    | xhdpi         | 240dpi-320dpi |
    | xxhdpi        | 320dpi-480dpi |
    | xxxhdpi       | 480dpi-640dpi |
    
    苹果的区分则更为简单：非高清屏、高清屏、超高清屏。


5. viewport
    手机浏览器是把页面放在一个虚拟的“窗口”（viewport）中，窗口可大于或小于手机的可视区域，一般手机默认viewport大于可视区域。这样不会破坏没有针对手机浏览器优化的网页的布局，用户可以通过平移和缩放来看网页的其他部分。

    网页重构时使用的单位px，就是通常所说的像素，是网页设计中使用最多的长度单位。将显示器分成非常细小的方格，每个方格就是一个像素（这和我们理解的屏幕分辨率的1920px*1080px的px是不同的）。不同设置下，方格的大小不一样。

    例如iPhone4S如果不设置viewport，他就会默认是980px，就像把屏幕分成980份（不是屏幕分辨率的640px哦！）。如果设置一个元素为100px*100px，看起来就是屏幕的100/980。

    例如iPhone4S如果设置viewport，width=device-width，他就会是320px，就像把屏幕分成320份（不是屏幕分辨率的640px哦！）。如果设置一个元素为100px*100px，看起来就是屏幕的100/320。设置了viewport，width=device-width，弹出来的是设置好的宽度，375px、360px、320px。为什么是这个大小？这就要用到上面讲的知识点了。

    iPhone6的屏幕分辨率是1334*750px，ppi是326，所以系数是2x。那device-width就等于750/2=375px。

    红米1s的屏幕分辨率是1280*720px，ppi是312，所以系数是2x。那device-width就等于720/2=360px。

    页面里的红色块给的是200*200px，在几个设备看起来好像差不多大的样子。


## 2016.2.25

> **主要内容：**
	1. 巧用margin/padding的百分比值实现高度自适应
	2. CSS 小技巧
	
### **一、巧用margin/padding的百分比值实现高度自适应**
> css知识点：当margin/padding取形式为百分比的值时，无论是left/right，还是top/bottom，都是以父元素的width为参照物的！
优化方案是这样的：给容器设置padding-top/padding-bottom跟width一致的值（百分比）

```
#container {
    width: 50%;
    position: relative;
    background-color: red;
    overflow: hidden; //需要触发BFC消除margin折叠的问题
}
.placeholder:after {
    content: '';
    display: block;
    margin-top: 100%; //margin 百分比相对父元素宽度计算
} 
img {
    position: absolute;
    top: 0;
    width: 100%;
}
```

### **二、CSS 小技巧**

1. 使用:not()添加/去除导航上不需要的边框

    ```
    .nav li:not(:last-child) {
         border-right: 1px solid #666;
    }
    ```

2. 为body添加行高：文本元素可以很容易从body继承

    ```
    body {
         line-height: 1;
    }
    ```

3. 垂直居中任何元素

    ```
    html, body {
      height: 100%;
      margin: 0;
    }
     
    body {
      -webkit-align-items: center;  
      -ms-flex-align: center;  
      align-items: center;
      display: -webkit-flex;
      display: flex;
    }
    ```

4. 逗号分离的列表

    ```
    ul > li:not(:last-child)::after {
      content: ",";
    }
    ```

5. 使用负 nth-child 选择元素

    ``` 
    /* 选择1到3的元素并显示 */
    li:nth-child(-n+3) { 
       display: block;
    }
    ```

6. 继承box-sizing

    ```
    html {
      box-sizing: border-box;
    }
     
    , :before, *:after {
      box-sizing: inherit;
    }
    ```

7. 表格单元格等宽

    ```
    .calendar {
      table-layout: fixed;
    }
    ```

8. 使用属性选择器选择空链接

    ```
    a[href^="http"]:empty::before {
      content: attr(href);
    }



## 2016.2.24

> **主要内容：**  
	1. 30行代码实现Javascript中的MVC  
	2. 17行代码实现的简易Javascript字符串模板  
	3. 优雅的数组降维——Javascript中apply方法的妙用  
	
### **一、30行代码实现Javascript中的MVC**

**(一) 什么是MVC：** MVC模式是软件工程中一种软件架构模式，一般把软件模式分为三部分，模型(Model)+视图(View)+控制器(Controller)

* **模型：**模型用于封装与应用程序的业务逻辑相关的数据以及对数据处理的方法。模型有对数据直接访问的权利。模型不依赖 “视图” 和 “控制器”, 也就是说模型它不关心页面如何显示及如何被操作.
* **视图：**视图层最主要的是监听模型层上的数据改变，并且实时的更新html页面。当然也包括一些事件的注册或者ajax请求操作(发布事件),都是放在视图层来完成。
* **控制器：**控制器接收用户的操作，最主要是订阅视图层的事件，然后调用模型或视图去完成用户的操作;比如：当页面上触发一个事件，控制器不输出任何东西及对页面做任何处理;它只是接收请求并决定调用模型中的那个方法去处理请求,然后再确定调用那个视图中的方法来显示返回的数据。

**(二)  代码实现**

1. MVC的基础是观察者模式，这是实现model和view同步的关键
    > 为了简单起见，每个model实例中只包含一个primitive value值。

    ```
    function Model(value) {
        this._value = typeof value === 'undefined' ? '' : value;
        this._listeners = [];
    }
    Model.prototype.set = function (value) {
        var self = this;
        self._value = value;
        // model中的值改变时，应通知注册过的回调函数
        // 按照Javascript事件处理的一般机制，我们异步地调用回调函数
        // 如果觉得setTimeout影响性能，也可以采用requestAnimationFrame
        setTimeout(function () {
            self._listeners.forEach(function (listener) {
                listener.call(self, value);
            });
        });
    };
    Model.prototype.watch = function (listener) {
        // 注册监听的回调函数
        this._listeners.push(listener);
    };
    
    
    // html代码：
    <div id="div1"></div>
    // 逻辑代码：
    (function () {
        var model = new Model();
        var div1 = document.getElementById('div1');
        model.watch(function (value) {
            div1.innerHTML = value;
        });
        model.set('hello, this is a div');
    })();
    ```

    借助观察者模式，我们已经实现了在调用model的set方法改变其值的时候，模板也同步更新，但这样的实现却很别扭，因为我们需要手动监听model值的改变（通过watch方法）并传入一个回调函数，有没有办法让view（一个或多个dom node）和model更简单的绑定呢？

2. 实现bind方法，绑定model和view

    ```
    Model.prototype.bind = function (node) {
        // 将watch的逻辑和通用的回调函数放到这里
        this.watch(function (value) {
            node.innerHTML = value;
        });
    };
    
    
    // html代码：
    <div id="div1"></div>
    <div id="div2"></div>
    // 逻辑代码：
    (function () {
        var model = new Model();
        model.bind(document.getElementById('div1'));
        model.bind(document.getElementById('div2'));
        model.set('this is a div');
    })();
    ```

    通过一个简单的封装，view和model之间的绑定已经初见雏形，即使需要在一个model上绑定多个view，实现起来也很轻松。注意bind是Function类prototype上的一个原生方法，不过它和MVC的关系并不紧密，笔者又实在太喜欢bind这个单词，一语中的，言简意赅，所以索性在这里把原生方法覆盖了，大家可以忽略。言归正传，虽然绑定的复杂度降低了，这一步依然要依赖我们手动完成，有没有可能把绑定的逻辑从业务代码中彻底解耦呢？

3. 实现controller，将绑定从逻辑代码中解耦

    细心的朋友可能已经注意到，虽然讲的是MVC，但是上文中却只出现了Model类，View类不出现可以理解，毕竟HTML就是现成的View（事实上本文中从始至终也只是利用HTML作为View，javascript代码中并没有出现过View类），那Controller类为何也隐身了呢？别急，其实所谓的”逻辑代码”就是一个框架逻辑（姑且将本文的原型玩具称之为框架）和业务逻辑耦合度很高的代码段，现在我们就来将它分解一下。

    如果要将绑定的逻辑交给框架完成，那么就需要告诉框架如何来完成绑定。由于JS中较难完成annotation（注解），我们可以在view中做这层标记——使用html的标签属性就是一个简单有效的办法。

    ```
    function Controller(callback) {
        var models = {};
        // 找到所有有bind属性的元素
        var views = document.querySelectorAll('[bind]');
        // 将views处理为普通数组
        views = Array.prototype.slice.call(views, 0);
        views.forEach(function (view) {
            var modelName = view.getAttribute('bind');
            // 取出或新建该元素所绑定的model
            models[modelName] = models[modelName] || new Model();
            // 完成该元素和指定model的绑定
            models[modelName].bind(view);
        });
        // 调用controller的具体逻辑，将models传入，方便业务处理
        callback.call(this, models);
    }
    
    
    // html:
    <div id="div1" bind="model1"></div>
    <div id="div2" bind="model1"></div>
    // 逻辑代码：
    new Controller(function (models) {
        var model1 = models.model1;
        model1.set('this is a div');
    });
    ```

    就这么简单吗？就这么简单：在Controller中完成业务逻辑并对Model进行修改，Model的变化触发View的自动更新，怎么样，算得上一个有模有样的MVC吧？当然，这样的”框架”还不足以用于生产环境，不过如果它能或多或少地帮助到大家对于MVC的理解的话，博主就非常满足了。

    整理后去掉注释的”框架”代码：

    ```
    function Model(value) {
        this._value = typeof value === 'undefined' ? '' : value;
        this._listeners = [];
    }
    Model.prototype.set = function (value) {
        var self = this;
        self._value = value;
        setTimeout(function () {
            self._listeners.forEach(function (listener) {
                listener.call(self, value);
            });
        });
    };
    Model.prototype.watch = function (listener) {
        this._listeners.push(listener);
    };
    Model.prototype.bind = function (node) {
        this.watch(function (value) {
            node.innerHTML = value;
        });
    };
    function Controller(callback) {
        var models = {};
        var views = Array.prototype.slice.call(document.querySelectorAll('[bind]'), 0);
        views.forEach(function (view) {
            var modelName = view.getAttribute('bind');
            (models[modelName] = models[modelName] || new Model()).bind(view);
        });
        callback.call(this, models);
    }
    ```

4. 一个简单的例子

    下面请大家看一个简单例子，如何实现电子表

    ```  
    // html:
    <span bind="hour"></span> : <span bind="minute"></span> : <span bind="second"></span>
    // controller:
    new Controller(function (models) {
        function setTime() {
            var date = new Date();
            models.hour.set(date.getHours());
            models.minute.set(date.getMinutes());
            models.second.set(date.getSeconds());
        }
        setTime();
        setInterval(setTime, 1000);
    });
    ```

    可以看出，controller中只负责更新model的逻辑，和view完全解耦；而view和model的绑定是通过view中的属性和框架中controller的初始化代码完成的，也没有出现在业务逻辑中；至于view的更新，也是通过框架中的观察者模式实现的。

### **二、17行代码实现的简易Javascript字符串模板**

**例子：**

```
render('My name is {name}'， {
  name: 'hsfzxjy'
}); // My name is hsfzxjy
 
render('I am in {profile.location}', {
  name: 'hsfzxjy',
  profile: {
    location: 'Guangzhou'
  }
}); // I am in Guangzhou
 
render('{ greeting }. \\{ This block will not be rendered }', {
  greeting: 'Hi'
}); // Hi. { This block will not be rendered }
```

**代码：**

```
function render(template, context) {
  var tokenReg = /(\\)?\{([^\{\}\\]+)(\\)?\}/g;
  return template.replace(tokenReg, function (word, slash1, token, slash2) {
    if (slash1 || slash2) { 
      return word.replace('\\', ''); // 如果分隔符被转义，则不渲染
    }
    var variables = token.replace(/\s/g, '').split('.');
    var currentObject = context;
    var i, length, variable;
    for (i = 0, length = variables.length, variable = variables[i]; i < length; ++i) {
      currentObject = currentObject[variable];
      if (currentObject === undefined || currentObject === null) 
        return '';
    }
    return currentObject;
  })
}
```


### **三、优雅的数组降维——Javascript中apply方法的妙用**

```
function reduceDimension(arr) {
    return Array.prototype.concat.apply([], arr);
}
```


## 2016.2.23

> **主要内容：**  
	1. 前端图片选择  
	2. js运算符中的一些小技巧  
	
### **一、前端图片选择**

1. 关于jpg
    由于其可压缩的特点，对于背景颜色较为复杂（比如照片等图）并且没有透明的图片，建议采用jpg。这样在保证了图片肉眼几乎看不出很大区别的情况下，把图片压得更小，压缩更快。不过对于有线条及文字的内容，不推荐用jpg。
1. 关于gif
    如果需要动图的话，由于APNG对兼容性对不友好gif依然还是首选。
1. 关于png
    * png8+alpha可以加入日常的开发中。对于桌面端，在不用考虑ie6的情况下，alpha透明的png8可以用在一些图片颜色较为简单的地方。对于移动端，可以考虑直接采用alpha透明的png8，而不采用png32，因为移动端的网络相较pc端较差，因此，采用png8+alpha可以节省流量。
	* png32在桌面端中，还是可以作为主要的图片格式。因为桌面端相较于移动端，网速更友好，同时，显示器的浏览对于图片的精细程度要求更高，因此，一些比较复杂的按钮，logo还是应当采用png32来处理
	* png8+索引透明可以用来处理桌面端对于低版本浏览器的（ie6）的兼容问题，虽然采用背景杂边的方式只能解决部分锯齿问题，但总好过于无。ie6已然是很早之前的浏览器，本身对其的兼容就势必会牺牲一些东西。因此，个人感觉还是对于背景简单的，直接采用杂边的方式来解决，而对于背景较为复杂的，目前我也只能想到采用去掉杂边的方法去解决（其实也就是说锯齿直接不管了）。

### **二、js运算符中的一些小技巧**

1. %取余运算符：运算结果的正负号由第一个运算子的正负号决定

2. +运算符：当运算子中出现对象的时候，先调用该对象的valueOf方法。如果返回结果为原始类型的值，则转换为字符串；否则继续调用该对象的toString方法，然后转换为字符串。此外，当+运算符作为数值运算符放在其他值前面的时候，可以用于将任何值转为数值。

    ```
    var now = new Date();
    typeof (now + 1) // "string"
    typeof (now - 1) // "number"
    
    1 + [1,2]
    // "11,2"
    1 + {a:1}
    // "1[object Object]"
    
    {a:1} + 1  // 1
    ({a:1})+1  //"[object Object]1"
    //解释：{a:1}被当做了代码块处理，而这个代码块没有返回值，所以整个表达式就返回1了。但是放在了圆括号中的{a:1}，因为js预期()中是一个值，所以它就又被当做对象处理了。
    
    [] + [] // ""
    [] + {} // "[object Object]"
    {} + [] // 0  解释：{}被视作代码块省略，+[]就是将[]转换为数值的意思了得到0.
    {} + {} // NaN
    
    ({}) + {} // "[object Object][object Object]"
    ({} + {}) // "[object Object][object Object]"
    
    //
    +true // 1
    +[] // 0
    +{} // NaN
    ```

3. ！取反运算符：!!x等同于Boolean(x)

    注：!!null 值是 false，其他的 object !!obj 值都是 true。

4. ~否运算符：~~x能够对小数取整，并且这是取整方法中最快的一种。

5. ^异或运算符：两次异或运算交换两个数的值

    ```
    var a = 10;
    var b = 99;
    
    a^=b, b^=a, a^=b;
    
    a // 99
    b // 10
    ```

6. << 左移运算符：<< 0可用于取整

7. >> 右移运算符：可以模拟2的整除运算
    
    ```
    5 >> 1 
    // 相当于 5 / 2 = 2
    
    21 >> 2 
    // 相当于 21 / 4 = 5
    
    21 >> 3 
    // 相当于 21 / 8 = 2
    
    21 >> 4 
    // 相当于 21 / 16 = 1
    ```

8. void运算符的作用是用来执行一个表达式，然后返回undefined，而且它的运算符优先级也比较高void 4+7 实际上等同于 (void 4) +7。

9. 一般运算符是左结合的，但是=和三目运算符？：却是右结合的

    ```
    w = x = y = z;
    q = a?b:c?d:e?f:g;
    //相当于：
    w = (x = (y = z)); 
    q = a?b:(c?d:(e?f:g));
    ```


## 2016.2.22

> **主要内容：**  
	1. IE和firefox浏览器的event事件兼容性汇总  
	2. 一道常被人轻视的前端JS面试题  
	
### **一、IE和firefox浏览器的event事件兼容性汇总**

* IE中可以直接使用event对象，但是Mozilla不可以直接使用。
* 事件来源：event.target[Moz]与event.srcElement[IE]，但后者是返回一个Html Element
* 键盘值的取得：event.which[Moz]与event.keyCode[IE]
* 鼠标点击的绝对位置：event.pageX,event.pageY[Moz]与event.x,event.y[IE]
* 鼠标点击的相对位置：event.layerX,event.layerY[Moz]与event.offsetX,event.offsetY[IE]
* 事件绑定：addEventListener，removeEventListener[Moz]与attachEvent，detatchEvent[IE]
* IE中要在事件前加on,而在Moz中不能加

### **二、一道常被人轻视的前端JS面试题**

> [答案解答网址](http://www.cnblogs.com/xxcanghai/p/5189353.html)

```
function Foo() {
    getName = function () { alert (1); };
    return this;
}
Foo.getName = function () { alert (2);};
Foo.prototype.getName = function () { alert (3);};
var getName = function () { alert (4);};
function getName() { alert (5);}
 
//答案：
Foo.getName();//2
getName();//4
Foo().getName();//1
getName();//1
new Foo.getName();//2
new Foo().getName();//3
new new Foo().getName();//3
```


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
    * duck type：比如不知道这个是不是数组，可以判断他的特征：length数字，是否有joying,push等等

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