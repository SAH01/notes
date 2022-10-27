# layui实现单页面弹出层

---

首先需要导入layui的js和css：

```html
<link rel="stylesheet" href="layui/css/layui.css" />
<script src="layui/layui.js"></script>
```

实现效果如下所示：单击政策图解按钮，会弹出一个子页面

![image-20221019215024652](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221019215024652.png)

![image-20221019215055111](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221019215055111.png)

该layui弹出层实现代码如下：

## 1、首先需要一个按钮

```html
<button onclick="selectRole()" style="margin-top: 30px;"  class="layui-btn layui-btn-normal">政策图解</button>
```

## 2、然后给这个button绑定layui事件

**content:$("#popSearchRoleTest").html()**

这段代码就是第三步提到的那个div块的id值，这是要获取那段html代码的文本然后在弹出的子页面做展示。

```javascript
//政策图解弹层
function selectRole(){
    layer.open({
        //layer提供了5种层类型。可传入的值有：0（信息框，默认）1（页面层）2（iframe层）3（加载层）4（tips层）
        type:1,
        title:"政策图解",
        area: ['85%','80%'],
        content:$("#popSearchRoleTest").html()
    });
}
```

## 3、写出html代码，这段代码就是一会要显示的部分

注意设置为**style="display:none;"**

```html
<div class="layui-row" id="popSearchRoleTest" style="display:none;">
	<table lay-skin="row" class="layui-table" >
	<thead>
        <tr>
            <th style="text-align: center">序号</th>
            <th style="text-align: center">图解政策标题</th>
            <th style="text-align: center">发布时间</th>
            <!--<th>签名</th>-->
        </tr>
    </table>
</div>
```

最后点击按钮就会在当前页面弹出的一个子页面中，显示出这段代码所呈现的样式。