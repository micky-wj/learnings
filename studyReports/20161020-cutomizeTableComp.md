## 2016-10-20 自定义table组件实现

## 固定表头的实现
1. 父容器设置`width，overflow：hidden`
1. 两个table，一个放表头，一个放内容，各自加入colgroup来对齐。
1. `.table-head`设置`width，overflow：hidden`
1. `.table-body`设置`height，overflow：scroll`
```html
<div class="table-con">
    <div class="table-head">
        <table>
            <colgroup>
                <col style="width: 80px;"/>
                <col style="width: 100px;"/>
            </colgroup>
            <thead>
                <tr><th>序号</th><th>内容</th></tr>
            </thead>
        </table>
    </div>
    <div class="table-body">
        <table>
            <colgroup>
                <col style="width: 80px;"/>
                <col style="width: 100px;"/>
            </colgroup>
            <tbody>
                <tr><td>1</td><td>我只是用来测试的</td></tr>
                <tr><td>2</td><td>我只是用来测试的</td></tr>
            </tbody>
        </table>
    </div>
</div>
<style>
    .table-con{width:100%;overflow:hidden;}
    .table-head{width:100%;padding-right:17px;overflow:hidden;}//纵向滚动条宽度
    .table-body{width:100%; height:300px;overflow:scroll;}
    .table-head table,.table-body table{width:100%;}
</style>
```
## 水平同时滚动的实现
> 监听table-body的滚动事件，将scrollLeft的负值赋值给table-head的margin-left。

```javascript
$('.table-body').scroll(function  (e) {
    const $target = $(e.currentTarget);
    $('.table-head').css('margin-left', - $target.scrollLeft());
});
```
## 列配置项
```javascript
{
    title: '标题'; // 设置标题
    width: '100px'; // 设置宽度
    dataIndex: 'data'; // 取得数组字段
    key: '1'; // 列的唯一标识key值
    tips: '提示信息'; // 提示信息
    auth: true; //是否有权限查看
    render: (text, record)=>{} //如何处理字段来显示
}
```