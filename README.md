bootstrap-pager
===========
![分页组件](https://raw.githubusercontent.com/bill1012/bootstrap-pager/master/image/pagination.png)

本组件主要使用在扁平风格页面的自定义分页，做到页面显示和分页组件分离。本分页组件在[JavaCode](http://code.admineap.com)中应用。

链家网房屋信息查询分页、拉钩大鲲项目的分页都使用类似的分页组件。

使用
=======

Step1： **引用样式**
```
<link rel="stylesheet" href="bootstrap/css/bootstrap.min.css">
<link rel="stylesheet" href="../css/bootstrap-pager.css">
```

Step2：**引用脚本**
```
<script src="jquery/jquery-1.8.3.min.js"></script>
<script src="bootstrap/js/bootstrap.min.js"></script>
<script src="../js/bootstrap-pager.js"></script>
```

Step3：**使用**

**静态分页**
```html
   <div id="page-container-static-normal"></div>
```
```javascript
   $("#page-container-static-normal").page({
         count:100,
         theme:"normal"
    });
    $("#page-container-static-normal").on("pageChanged",function (event,params) {
        console.log(params);
        $(this).data("page").refresh(params);
    })
```
**动态分页**

下面演示的是一个页面，在动态请求数据时，把分页信息发送到后台，并返回业务结果和分页信息，进行页面加载和分页组件渲染。
```javascript
  BaseBlog.prototype.init=function(){
    this.builder();
  }
  //加载数据，渲染页面
  BaseBlog.prototype.builder=function (params) {
        this.$element.html("");
        var self=this;
        this.pageSize=8;
        var pageInfo=params||{pageNum:1,pageSize:this.pageSize};
        ajaxPost(this.options.url,{pageInfo:JSON.stringify(pageInfo),key:$("#search_value").val()},function (data) {
            self.data=data;
        })
        this.buildByData(this.data.data);
        this.buildPager(this.data.pageInfo);
    }
//设置分页
BaseBlog.prototype.buildPager = function (pageInfo) {
        var self=this;
        self.pager=null;
        var pager = $("#" + this.options.pageContainer).data("page").refresh(params);
        pager.$element.unbind("pageChanged");
        pager.$element.on("pageChanged", function (event, params) {
            self.$element.html("");
            self.builder(params);
        })
        this.pager = pager;
    }
```


参数和方法说明
===========
**参数说明**
 
 名称 | 默认值 | 说明
 ----|------|----
 count|  | 记录总数
 maxPage|9|显示的分页框数量
 pageSize|8|每页显示记录数
 theme|normal|可选`big`和`normal`，页面样式
 pageNum|1|当前页，随着用户操作变化
 
 **主要方法** 
 
 名称 | 参数 | 参数说明 | 方法说明
 ----|------|----|--------
 refresh|pageInfo|分页对象，含有pageNum和pageSize两个值|刷新分页
 init| | |初始化
 drawPage|count,pageNum|记录总数，当前页|分页渲染
 setPreviousButton| | |设置`上一页`按钮状态
 setNextButton| | |设置`下一页`按钮状态
 setActiveButton| | |高亮显示当前选中页
 bindEvent| | |为所有按钮绑定事件



