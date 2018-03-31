### 需求: 往`ul`里面添加10个`li`
##### 代码如下: 
```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <ul id="ul-test">
            
        </ul>
    </body>
    <script type="text/javascript">
        var oUl=document.getElementById("ul-test");
        for(var i=0;i<10;i++){
            var oLi=document.createElement('li');
            oLi.innerHTML=i;
            oUl.appendChild(oLi);
        }       
    </script>
</html>
```

#### 问题：相当于操作了10次DOM，如何优化，减少DOM的操作次数  
#### 两种解决方案
1. 使用 innerHTML
```javascript
var oUl=document.getElementById("ul-test");
//定义临时变量
var _html='';
for(var i=0;i<10;i++){
    //保存临时变量
    _html+='<li>'+i+'</li>'
}
//添加元素
oUl.innerHTML=_html;
```
2. 使用文档碎片: createDocumentFragment
```javascript
var oUl = document.getElementById("ul-test"),
    _frag = document.createDocumentFragment();
for(var i=0;i<10;i++){
    var oLi=document.createElement('li');
    oLi.innerHTML=i;
    //把元素添加进文档碎片
    _frag.appendChild(oLi);
}
//把文档碎片添加进元素
oUl.appendChild(_frag);
```
