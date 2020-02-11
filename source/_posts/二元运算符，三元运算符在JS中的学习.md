---
title: 二元运算符，三元运算符在JS中的学习
categories:
  - web
date: 2018-02-06 11:11:55
tags: [前端]
---

源于在扩展LayUI后台框架源码的tree模块时发现的，将其混淆的代码稍稍还原一下。



```javascript
layuiTree.prototype.tree = function($elem, a) { 

var self = this , options = self.options , nodes = a || options.nodes; 

layui.each(nodes, function(a, nodesF) { 

    var flag = nodesF.children && nodesF.children.length > 0 , $ul = $('<ul class="' + (nodesF.spread ? "layui-show" : "") + '"></ul>') , $li = $(\["<li " + (nodesF.spread ? 'data-spread="' + nodesF.spread + '"' : "") + " " + (nodesF.type ? 'data-type="' + nodesF.type + '"' : "") + ">", function() { return flag ? '<i class="layui-icon layui-tree-spread">' + (nodesF.spread ? icon.arrow\[1\] : icon.arrow\[0\]) + "</i>" : "" }(), function() { return options.check ? '<i class="layui-icon layui-tree-check">' + ("checkbox" === options.check ? icon.checkbox\[0\] : "radio" === r.check ? t.radio\[0\] : "") + "</i>" : "" }(), function() { return '<a href="' + (nodesF.href || "javascript:;") + '" ' + (options.target && nodesF.href ? 'target="' + options.target + '"' : "") + ">" + ('<i class="layui-icon layui-tree-' + (flag ? "branch" : "leaf") + '">' + (flag ? nodesF.spread ? icon.branch\[1\] : icon.branch\[0\] : icon.leaf) + "</i>") + ("<cite>" + (nodesF.name || "未命名") + "</cite></a>") }(), "</li>"\].join("")); flag && ($li.append($ul), self.tree($ul, nodesF.children)), $elem.append($li), "function" == typeof options.click && self.click($li, nodesF), self.spread($li, nodesF) ,options.drag && self.drag($li, nodesF) 
    }) 
}
```

满满的二元运算符，三元运算符有没有！！！

> **科普：**

&&和||返回的是两个操作数的的其中一个 &&，左操作数为真值时，返回右操作数，否则返回左操作数； ||,左操作数为真值时，返回左操作数，否则返回右操作数； &&和||还有短路原理，从左至右判断 。如果左边为假值时，不执行后面的表达式。 0、""、null、false、undefined、NaN、-0 在逻辑运算中为假值。

> **部分实例：**

如此精简的语句，完全可以优化老旧的if语句。

```
  if("function" == typeof a) {a()} elseif("function" == typeof b) {b()} else {c()} => "function" == typeof a ? a() : "function" == typeof b ?b() : c() if("function" == typeof a) a() => "function" == typeof a && a() if("function" != typeof b) a() => "function" == typeof b || a() if(flag1){if(flag2){a() }else{b() } }else{c() } => 'flag1 ? flag2 ? a() :b(): c()' if(!a) {var a = {}} => var a =a ||{} \[/code\] 还有一种if语句优化方式比较奇怪的 \[code lang="js"\] var b = 1 var a ={'0': function() {return 'true'}(),'1':true,'a':'baa'}\[b\] || 0; console.log(a)
```



> **总结：**

善用二元运算符,三元运算符和'function(){}()'立即执行函数，可以极大而高效的简便代码，减少代码量，同时伴随着代码可读性的降低，所以千万别忘了加上备注以便于后期维护。