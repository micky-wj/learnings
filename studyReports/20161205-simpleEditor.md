## 2016-12-5 简易富文本编辑器实现

> **两关键点：contenteditable与execCommand**

### 第一步：使内容框可编辑，增加contenteditable属性
```html
<div id="editor" contenteditable></div>
```
### 第二步：在样式按钮的点击事件回调中使用execCommand方法，注意再次点击后样式还原
主要用到documen.execCommand(command,show,value)方法，其中command表示要执行的命令，show表示是否向用户显示特定的对话框，value表示该命令的值。另一个比较常用的方法是window.getSelection() 获取选择区域。


常见命令如下，可参照[MDN的列表](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand)：
* 加粗命令：`document.execCommand(bold,false,null);`
* 斜体命令：`document.execCommand(italic,false,null);`
* 下划线命令：`document.execCommand(underline,false,null);`
* 左对齐命令：`document.execCommand(justifyleft,false,null);`
* 居中对齐命令：`document.execCommand(justifycenter,false,null);`
* 右对齐命令：`document.execCommand(justifyright,false,null);`
* 有序排序对齐命令：`document.execCommand(insertorderedlist,false,null);`
* 无序排序对齐命令：`document.execCommand(insertunorderedlist,false,null);`
* 删除选中的区块：`document.execCommand('Delete');`
* 剪下选中的区块：document.execCommand('Cut');`
* 上传图片命令：`document.execCommand(insertImage,false,src);`
* 添加链接命令：`document.execCommand(createLink,false,href);`
 
注：简易实现可以参照国外的[Summernote](https://github.com/summernote/summernote)，或是国内的[WangEditor](https://github.com/wangfupeng1988/wangEditor)。仔细看来确实坑比较多。
