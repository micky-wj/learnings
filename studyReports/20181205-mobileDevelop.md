# 移动端开发经验

## 一、样式相关

### 1. 适配方案：lib-flexible + px2rem-loader

* 安装npm包

    ```linux
    npm install px2rem-loader  lib-flexible --save
    ```

* 在main.js中引入lib-flexible

    ```javascript
    import 'lib-flexible/flexible.js'
    ```

* 在build下的 utils.js中，找到generateLoaders 方法，添加以下代码  

    ```javascript
    const px2remLoader = {
        loader: 'px2rem-loader',
        options: {
            remUnit: 37.5
        }
    }

    function generateLoaders (loader, loaderOptions) {
        const loaders = [cssLoader, px2remLoader]
        if (loader) {
            loaders.push({
            loader: loader + '-loader',
            options: Object.assign({}, loaderOptions, {
            sourceMap: options.sourceMap
            })
        })
    }
    ```

### 2. 初始加载loading动画

* 在渲染节点里增加loading元素

    ```html
    <div id="main">// vue里render的元素
        <div class="loading">
            <img src="<%=htmlWebpackPlugin.options.htmlAssetPrefix %>/static/imgs/loading.gif" />
        </div>
    </div>
    ```

* 在head元素里追加loading的样式

    ```html
    <style>
    .loading {
        position: fixed;
        left: 0;
        top: 0;
        height: 100vh;
        width: 100vw;
        padding-top: 50%;
        background: #ffca0d;
    }
    .loading img {
        width: 100%;
    }
    </style>
    ```

## 二、开发相关

### 1. vue支持jsx

* 安装插件

    ```linux
    npm install babel-plugin-syntax-jsx babel-plugin-transform-vue-jsx babel-helper-vue-jsx-merge-props babel-preset-env --save-dev
    ```

* .babelrc增加如下配置：

    ```json
    {
        "presets": ["env"],
        "plugins": ["transform-vue-jsx"]
    }
    ```

* vue的render传入参数h即可使用

    ```javascript
    render (h) {
       // cfg为外部配置常量
        return (
            <div>
                {cfg && cfg.render.apply(null, [h, this.data])}
            </div>
        )
    }
    ```

### 2. vue支持mock

* 安装插件

    ```linux
    npm install axios-mock-adapter --save-dev
    ```

* .babelrc增加如下配置：

    ```javascript
    import axios from 'PLUGINS/http'// http对外常量
    import MockAdapter from 'axios-mock-adapter'

    import example1 from './example1'
    import example2 from './example2'

    export default {
        init () {
            const Mock = new MockAdapter(axios)
            example1(Mock)
            example2(Mock)
        }
    }
    ```

* 在main.js引入并安装

    ```javascript
    import mock from '@/mock/index'

    // 只有mock模式才启用
    if (ENV.isMockEnv) {
        mock.init()
    }
    ```

* example1文件使用示例：

    ```javascript
    export default function (Mock) {
        Mock.onGet(`/hehe`).reply(200, 'hehe')
        Mock.onGet(`/haha`).reply(200, 'haha')
    }
    ```

### 3. 打包成俩套html

* webpack生成环境打包文件，增加如下插件:

    ```javascript
    new HtmlWebpackPlugin({
        filename: path.resolve(__dirname, '../dist/index.html'),
        template: 'index.html',
        inject: false,
        htmlAssetPrefix: '/',
        ...others
    }),
    new HtmlWebpackPlugin({
        filename: path.resolve(__dirname, '../dist/test.html'),
        template: 'index.html',
        inject: false,
        htmlAssetPrefix: '/',
        ...others
    })
    ```

* index.html做如下修改：

    ```html
    <!DOCTYPE html>
    <html>
        <head>
        <link href="//at.alicdn.com/t/font_831101_ma9e50tmew7.css" rel="stylesheet">
        <% for (var css in htmlWebpackPlugin.files.css) { %>
        <link href="<%=htmlWebpackPlugin.options.htmlAssetPrefix %><%=htmlWebpackPlugin.files.css[css] %>" rel="stylesheet">
        <% } %>
        </head>

    <body>
        <div id="main"></div>
        <% for (var chunk in htmlWebpackPlugin.files.chunks) { %>
        <script type="text/javascript" src="<%=htmlWebpackPlugin.options.htmlAssetPrefix %><%=htmlWebpackPlugin.files.chunks[chunk].entry %>"></script>
        <% } %>
    </body>

    </html>
    ```

### 4. 增加debug工具

> [eruda](https://github.com/liriliri/eruda/blob/master/doc/README_CN.md)

* 安装插件

    ```linux
    npm install eruda --save
    ```

* window.onload的回调中，调用以下函数：

    ```javascript
    function addDebugTool = () => {
        if (/debug=true/.test(window.location)) {
            const script1 = document.createElement('script')
            const script2 = document.createElement('script')

            script1.type = 'text/javascript'
            script2.type = 'text/javascript'

            script1.src = '//cdn.bootcss.com/eruda/1.5.2/eruda.min.js'
            script2.innerHTML = 'window.eruda && eruda.init();'
            document.body.appendChild(script1)

            script1.onload = function () {
                document.body.appendChild(script2)
            }
        }
    }
    ```

## 四、组件相关

### 1. 方法调用modal

```javascript
import Vue from 'vue'
import modalTemplate from 'modalTemplate'
// 直接将Vue组件作为Vue.extend的参数
const Modal = Vue.extend(modalTemplate)// modalTemplate为modal的模板

let nId = 1
let $dom = null
export default {
    show ({
        type,
        data
    }) {
        let id = 'notice-' + nId++
        const instance = new Modal({})
        instance.vm = instance.$mount()
        instance.id = id
        instance.isShow = true
        instance.data = data
        instance.type = type
        // 插入dom 中
        $dom = instance.$el
        document.body.appendChild($dom)

        // 后插入的Modal组件z-index加一，保证能盖在之前的上面
        $dom.style.zIndex = nId + 1001
    },
    close () {
        if ($dom) {
            document.body.removeChild($dom)
            $dom = null
        }
    }
}
```

### 2. modal组件

```html
<template>
    <div class="modal-wrap">
    <div class="modal-mask" v-if="isShow" @click.stop.prevent="closeMyself"></div>
    <transition name="drop">
    <div class="modal-content" v-if="isShow" @click.stop.prevent>
        <h3 v-if="!!title">{{title}}</h3>
        <section>
        <slot>无内容</slot>
        </section>
        <aside @click.stop.prevent="closeMyself">
        <icon type="close"/>
        </aside>
    </div>
    </transition>
    </div>
</template>

<script>
import icon from 'CC/Icon'

export default {
    props: {
        isShow: {
            type: Boolean,
            default: false
        },
        title: {
            type: String,
            default: ''
        },
        'on-close': {
            type: Function,
            default: () => { }
        }
    },
    watch: {
        isShow (newVal) {
            if (newVal) {
                this.$utils.ModalHelper.disableScroll()
            } else {
                this.$utils.ModalHelper.enableScroll()
            }
        }
    },
    components: {
        icon
    },
    methods: {
        closeMyself (e) {
        this.$emit('on-close')
        }
    }
}
</script>
<style lang="stylus" type="stylesheet/stylus" scoped>
$modaHeaderlHeight = 80px // 弹窗头高度
.drop-enter-active {
    transition: all 0.5s ease-out;
}

.drop-leave-active {
    transition: all 0.3s ease-in;
}

.drop-enter {
    transform: translateY(-500px);
}

.drop-leave-active {
    transform: translateY(-500px);
}

.modal-wrap {
    position: fixed;
    width: 100%;
    height: 100%;
}

.modal-mask {
    background: #000;
    opacity: 0.5;
    z-index: 5;
    width: 100%;
    height: 100%;
    position: fixed;
    left: 0;
    top: 0;
}

.modal-content {
    border-radius: 8px;
    border: none
    position: fixed;
    top: 20%;
    left: 50%;
    box-shadow: 0 0 10px #bbb;
    width: 80%;
    height: 60%;
    overflow: hidden;
    background: #fff;
    margin-left: -40%;
    z-index: 10;
    padding: 30px;

    &>h3 {
        border-bottom: 2px solid #e8e8e8;
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        font-weight: 700;
        text-align: center;
        line-height: $modaHeaderlHeight;
        background: rgba(255, 255, 255, 0.9);
        z-index: 1;

        &+section {
            padding-top: $modaHeaderlHeight;
        }
    }

    &>section {
        height: 100%;
        scrollBetter();
    }

    &>aside {
        position: absolute;
        top: 0;
        right: 30px;
        line-height: $modaHeaderlHeight;
        color: #a1a1a1;
        text-align: center;
        cursor: pointer;
        z-index: 2;

        &:hover, &:active {
            color: #a1a1a1;
        }
    }
}
</style>
```

## 五、兼容相关

### 1. 修复：iOS 下键盘唤出后，fixed 元素失效

> 将滚动元素包裹起来，仅在其包裹元素内部滚动

```html
// html
<header class="top"></header>
<section class="main">
    <div class="scrollArea"></div>
</section>

// css
<style>
.top {
    position: fixed;
    top: 0;
    left: 0;
    height: 50px;
}
.main {
    position: absolute;
    top: 50px;
    left: 0;
    bottom: 180px; 
    height: calc(100vh - 50px);
    width: 100%;
    -webkit-overflow-scrolling: touch; 
}
</style>
```

### 2. 修复：钉钉分享链接时，获取的是初始href

> 只能在每次路由更改后，重新渲染页面。在router增加如下函数：

```javascript
router.beforeEach((to, from, next) => {
    const refreshHref = (href) => {
        window.location.href = href
        location.reload()
    }
    // 为了兼容钉钉，分享链接时，拿到的是最初始的href
    if (
        navigator.userAgent.toLowerCase().indexOf('dingtalk') > -1 &&
        to.name && from.name &&
        to.name !== from.name
    ) {
    // 防抖
        _.debounce(refreshHref, 150)(location.href.split('#')[0] + '#' + to.fullPath)
    }
     next()
})
```

### 3. 修复：弹出浮层后，仍能滚动被遮挡的元素

* 弹出时调用ModalHelper.disableScroll()，关闭弹窗调用ModalHelper.enableScroll()

    ```javascript
    \\ js
    export const ModalHelper = ((bodyCls) => {
    let scrollTop
    return {
        disableScroll () {
        scrollTop = document.scrollingElement.scrollTop
        document.body.classList.add(bodyCls)
        document.body.style.top = -scrollTop + 'px'
        },
        enableScroll () {
        document.body.classList.remove(bodyCls)
        // scrollTop lost after set position:fixed, restore it back.
        document.scrollingElement.scrollTop = scrollTop
        }
    }
    })('modal-open')

    \\ css
    body.modal-open {
        position: fixed;
        width: 100%;
    }
    ```

### 4. 元素设置了border-radius

> 注意：指定边框圆角，若无边框，得设置border: none。否则会被添加默认边框

### 5. 元素设置了position: absolute

> 注意：需指定z-index，否则会被文本遮挡

### 6. ios滚动更顺畅

```css
-webkit-overflow-scrolling: touch;
```

### 7. 页面窗口自动调整到设备宽度，并且禁止用户缩放

* 增加以下meta

    ```html
    <meta name="viewport" content="width=device-width,maximum-scale=1.0,minimum-scale=1.0,initial-scale=1.0, user-scalable=no">
    ```
* window.onload的回调函数，调用以下函数

    ```javascript
    function disableScale = () => {
        document.addEventListener('touchstart', function (event) {
            if (event.touches.length > 1) {
            event.preventDefault()
            }
        })

        let lastTouchEnd = 0
        document.addEventListener('touchend', function (event) {
            const now = (new Date()).getTime()
            if (now - lastTouchEnd <= 300) {
                event.preventDefault()
            }
            lastTouchEnd = now
        }, false)

        document.addEventListener('gesturestart', function (event) {
            event.preventDefault()
        })
    }
    ```

## 六、优化相关

### 1. 页面缓存

* route的meta里增加keepAlive属性，需要被缓存则为true

    ```javascript
    routes: [{
        path: '/',
        name: 'list',
        component: List,
        meta: {
            // 标识此组件需要被缓存
            keepAlive: true
        }
    }
    ```

* 在App.vue 修改如下:

    ```html
    <div id="app">
        <keep-alive>
        <router-view v-if="$route.meta.keepAlive"></router-view>
        </keep-alive>
        <router-view v-if="!$route.meta.keepAlive"></router-view>
    </div>
    ```

### 2. 切换页面时，都将滚动条置顶

* router里增加scrollBehavior

    ```javascript
    scrollBehavior (to, from, savedPosition) {
        return {
            x: 0,
            y: 0
        }
    }
    ```
  
### 3. 返回按钮

> 点击返回按钮后的回调如下：

```javascript
goBack () {
    // 关闭loading
    Indicator.close()
    // 若无历史，则走route的meta里设置的backurl
    if (window.history.length <= 1) {
        let url = '/'
        const getBackURL = _.get(this.$route.meta, 'getBackURL')

        if (_.isFunction(getBackURL)) {
            url = getBackURL(this.$route)
        }
        this.$router.push(url)
    } else {
        this.$router.go(-1)
    }
}  
```

### 4. 保存滚动位置

```javascript
watch: {
    '$route' (to, from) {
        if (to.name === 'list' && to.meta.scrollTop) {
            document.getElementById('scroll-list').scrollTop = to.meta.scrollTop
            to.meta.scrollTop = 0
        }
    }
},
beforeRouteLeave (to, from, next) {
    from.meta.scrollTop = document.getElementById('scroll-list').scrollTop
    next()
}
```