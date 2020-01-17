---
title: FormData上传图片通用办法
url: 104.html
id: 104
categories:
  - web
date: 2017-09-26 12:28:12
tags: [前端]
---

关于FormData 对象的使用。 FormData 对象，使用浏览器原生特性实现文件上传，可以把form中所有表单元素的name与value组成一个queryString，提交到后台。在使用Ajax提交时，使用FormData对象可以减少拼接queryString的工作量。 通过FormData传输的数据格式和表单通过submit() 方法传输的数据格式相同 重要属性：

> enctype="multipart/form-data"

默认样式看完就知道自定义样式的重要性了，常用解决办法：

>  
>    上传文件
>    

美化样式：clip:rect(0 0 0 0);实现隐藏lable标签。btn-primary参照bootstrap样式. 另一种美化方式可参考[http://www.haorooms.com/post/css\_input\_uploadmh](http://www.haorooms.com/post/css_input_uploadmh). **一：使用FormData结合表单一并提交** 1.创建一个FormData空对象，然后使用append方法添加key/value。

>  
> 
> name:
> gender:  male female
> photo:

name值为与后台交互文件参数值需要保持一致;或可以不使用name值，在下面的方法中单独是用formData.append方法。 上传方式：

> function fsubmit(){
>   var formElement = document.querySelector("form1");
>   var outOfFormValue= document.querySelector("outOfForm").value ;
>   var formData = new FormData(formElement);
>   var request = new XMLHttpRequest();
>   request.open("POST", "submit.php");
>   formData.append("outOfFormValue", outOfFormValue);
>   request.send(formData);
> }

outOfFormValue：这样你就可以在发送请求之前自由地附加不一定是用户编辑的字段到表单数据里。 **二：使用ajax异步上传返回服务器图片地址赋值表单一并提交** 使用场景主要是现在许多第三方云服务商提供的对象存储，时序化数据库，包括阿里，腾讯，七牛云等等，针对企业客户场景进行灵活地部署和运维。 原理其实是一样的，只是处理的流程不一样；

> 

通过隐藏的iframe来隐藏form表单提交的页面刷新的小bug。 点击提交,因为target=iframe，监听iframe的load事件，能获取到上传到了iframe中的返回值，进行进一步处理。

> $('iframe').on('load', function() {
> var responseText = $('iframe')\[0\].contentDocument.body.textContent;
> var responseData = JSON.parse(responseText) || {};
> if (responseData.isSuccess == true || responseData.code == 200) {
> //success
> } else {
> //error 
> }
> });

再将返回的img的URL值赋值新的form表单中，重新点击提交完成操作。 第一次写，不是很完整，也没有demo，还望海涵。