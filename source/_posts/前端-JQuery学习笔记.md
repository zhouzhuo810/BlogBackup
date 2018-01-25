---
title: 前端-JQuery学习笔记
date: 2018-01-25 15:18:49
tags:
  - 网页前端
categories: 网页前端
---

## 常见问题

### checkbox类型的input的取值、赋值和监听
插件推荐：[https://github.com/flatlogic/awesome-bootstrap-checkbox](https://github.com/flatlogic/awesome-bootstrap-checkbox)
```javascript
//全选逻辑
    $("#checkbox-all").change(function () {
        //是否选中
        var isCheck = $(this).is(':checked');
        //jquery1.6之后使用.prop而不是.attr来选中或取消选中
        $(".checkbox-child").prop("checked", isCheck);
    });
```

<!-- more -->

### 表格添加id隐藏列
```
class="checkbox-child data-id hide"
```

### 表格获取选中行的id
```javascript
/*获取选中的行数*/
function getChooseRowsCount() {
    var count = 0;
    var trs = $("#project-list").find("tr");
    for (var i = 0; i < trs.length; i++) {
        var row = trs.eq(i);
        var cb = row.find(":checkbox");
        var isCheck = cb.is(':checked');
        if (isCheck) {
            count++;
        }
    }
    return count;
}
/*获取选中行的数据库id，多个用逗号隔开*/
function getChooseRowsDbIds() {
    var ids = "";
    var trs = $("#project-list").find("tr");
    for (var i = 0; i < trs.length; i++) {
        var row = trs.eq(i);
        var cb = row.find(":checkbox");
        var isCheck = cb.is(':checked');
        if (isCheck) {
            var colDbId = row.find("td.data-id");
            ids += colDbId.text();
            if (i < getChooseRowsCount() - 1) {
                ids += ",";
            }
        }
    }
    return ids;
}
```

### twbsPagination页码不刷新问题

相关插件：[https://github.com/esimakin/twbs-pagination](https://github.com/esimakin/twbs-pagination)
```javascript
//先销毁，保证总页码动态
$("#page-indicator").twbsPagination("destroy");
//分页绑定
$('#page-indicator').twbsPagination({
    totalPages: data.totalPage,
    visiblePages: 10,
    onPageClick: function (event, page) {
        //全选取消
        $("#checkbox-all").prop("checked", false);
        //重载数据
        var mid = localStorage.getItem("userId");
        //回调里面不要再调用销毁和绑定方法，否则死循环。
        justUpdateList(mid, page);
    }
});
```