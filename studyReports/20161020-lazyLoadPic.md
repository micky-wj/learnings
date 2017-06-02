## 2016-10-20 图片懒加载实现

1. HTML：增加img元素
	* 增加data-src属性，传入实际图片地址
	* 增加src属性，传入1px透明图片地址

2. CSS：width和height传入固定值
3. JS：图片onload后更改src值

 ``` javascript
function LoadImgs() {
    var me = this;
    var imgs = me.$el.find('[data-src]');
    var loadImage = function (src, $img, callback) {
        var mockImg = new Image();
        mockImg.src = src;
        if (mockImg.complete) { // 如果图片已经存在于浏览器缓存，直接调用回调函数
            callback.call($img, src);
            return; // 直接返回，不用再处理onload事件
        }
        mockImg.onload = function () {
            mockImg.onload = null;
            callback.call($img, src);
        }
    };
    for (var i = 0,
        len = imgs.length; i < len; i++) {
        var $img = $(imgs[i]);
        var src = $img.attr('data-src');
        loadImage(src, $img,
            function (src) {
                this.attr('src', src)
            });
    }
}
```
