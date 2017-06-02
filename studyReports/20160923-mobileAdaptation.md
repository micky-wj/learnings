## 2016-9-23 移动端适配技巧
rem+dp适配方案概要：
> [移动端适配技巧.ppt](https://github.com/micky-wj/learnings/blob/master/files/MobileAdaptation.ppt.ppt)  
[参考：淘宝适配方案](http://www.w3cfuns.com/notes/23659/5e3cd2904a56f5e6b86c4d49e90e0f34.html)

### 一、准备工作
1. 动态改写`<meta>`标签（scale相关值） `scale = 1 / dpr`
2. 给`<html>`元素添加`data-dpr`属性，并且动态改写`data-dpr`的值
3. 给`<html>`元素添加`font-size`属性，并且动态改写`font-size`的值
    * device-width = 设备的物理分辨率/(dpr * scale)
    * font-size = deviceWidth / 10

###二、实际操作

1. 把视觉稿中的px转换成rem
    * css尺寸 = 标注尺寸/设计稿横向分辨率/10

2. font-size需要额外的媒介查询，不使用rem
    * 使用`[data-dpr]`属性来区分不同dpr下的文本字号大小
