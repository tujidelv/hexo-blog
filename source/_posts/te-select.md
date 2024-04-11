---
title: 可输入可选择可模糊查询的 select 下拉
date: 2018-12-02 19:00:12
categories:
- 前端
- 经典示例
tags:
- html
- css
- javascript
---

# 可输入可选择可模糊查询的 select 下拉

## 目录

- [简介](#简介)
- [CSS 代码](#CSS代码)
- [JS 代码](#JS代码)
- [HTML 代码](#HTML代码)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

- 相比网上各种下拉插件，要么出现各种传值问题，要么样式跟现有的项目冲突，此方法没有选用 select 标签，返璞归真
- 啥也不说了，直接上代码。

## CSS代码

```css
<style>
.selectDiv {
	display:none;
	z-index:9999;
	position:absolute;/*不加的话可能会影响页面其他元素*/
	border:1px solid #999;
	width:215px;/*调整下拉框的宽度*/
	height:106px;
	margin-left:74px;
	background:white;
	border-radius:4px;/*调整边框圆角*/
	overflow-y:scroll;/*添加下拉框滚动条*/
	overflow-x:hidden;/*防止ie上下拉框出现横向滚动条*/
} 
.selectSpan {
 	width:190px;/*调整下拉框每个元素的宽度*/
 	height:18px;
 	padding-top:3px;
 	border:1px solid #ffffff;
 	margin-left:4px;
 	display:block;/*很重要，调整宽高度需要*/
 	word-wrap:break-word;/*自动换行*/
 	vertical-align:middle;/*垂直居中*/
 } 
.selectSpan:hover {
 	background:#1f7ed0;
 }
</style>
```

## JS代码

```javascript
var TempArr = [];
var selectDivHtml = "";
// 当鼠标点击输入框时执行此方法
function queryUnCreateAddress(){
	TempArr = [];
	var showFlag = true;
	$("#selectDivShow").html(selectDivHtml);

	$.ajax({ //此为向数据库查询json数据填充到下拉框里
		type:"post",
		url:path + "/workplace/workplaceInformationMessage!queryUnCreateAddress.action",
		dataType:"json",
		async:false,
		success:function(data){
			TempArr = data;
		    if(null != data && data.length > 0){
		    	$(data).each(function(index,value){
		    		var leng = checkLength(value.workplaceAddress); //计算数据的长度，区分中英文
					var height = Math.ceil(leng/21) * 15; //设置下拉框每个选项的高度，
					if(height == 15){
						height = 18;
					}
	    			selectDivHtml +="<span class='selectSpan' style='height:"+height+"px' data-code='"+value.addressCode+"' data-address='"+value.workplaceAddress+"' onclick='selectedValueInput(this)'>"+value.workplaceAddress+"</span>";
		    	});
		    }else{
		    	showFlag = false;
		    }
		},
		error:function(){
			alert("系统错误");
		}
	});
	$("#selectDivShow").append(selectDivHtml);
	selectDivHtml = "";

	if(showFlag){
		$("#selectDivShow").attr("style","display:block");
	}
}
// 点击下拉框每个选项时触发此方法
function selectedValueInput(obj){
	var code = $(obj).data("code");
	var address = $(obj).data("address");
	$("#workplaceAddressNum").val(code); //隐藏域，按需添加，方便传值到后台
	$("#contractAddress").val(address);
	$("#selectDivShow").attr("style","display:none");
}
// 在输入框输入内容时触发此方法，方便实时监听匹配
function setinput(this_){
	var select = $("#selectDivShow");
	select.html("");

	for(var i = 0;i<TempArr.length;i++){
		if(TempArr[i].workplaceAddress.substring(0,$(this_).val().length).indexOf($(this_).val()) == 0){
			var leng = checkLength(TempArr[i].workplaceAddress);
			var height = Math.ceil(leng/21) * 15;
			if(height == 15){
				height = 18;
			}
			selectDivHtml +="<span class='selectSpan' style='height:"+height+"px' data-code='"+TempArr[i].addressCode+"' data-address='"+TempArr[i].workplaceAddress+"' onclick='selectedValueInput(this)'>"+TempArr[i].workplaceAddress+"</span>";
		}
	}
	select.append(selectDivHtml);
	selectDivHtml = "";
}
// 离开输入框时触发此方法
function addressBlur(){
	var selectObj = $("#selectDivShow").find("span:hover");
	if(null != selectObj && selectObj.length > 0){ //针对鼠标点击下拉框选项触发此方法时的特殊处理
		selectedValueInput(selectObj);
		return;
	}
	$("#selectDivShow").attr("style","display:none");
}
// 计算输入值的长度
function checkLength(str){
	var sLen = 0;
	for(var i = 0;i<str.length;i++) {
		if(str.charAt(i) <= '\255') { // 单字节字符
			sLen++;
		}else { // 汉字或其他2字节字符
			sLen = sLen + 3;
		}
	}
	return sLen;
}
```

## HTML代码

```html
<input type="text" id="contractAddress" name="workplaceInformation.contractAddress" 
onblur="addressBlur()" oninput="setinput(this)" onfocus="queryUnCreateAddress()" value="${workplaceInformation.contractAddress }" />
<div id="selectDivShow" class="selectDiv"></div>
```

## 参考链接


## 结束语

- 20190723更新 仿携程等网站鼠标放入`目的地`输入框时的样式
```css
<style>
.labels {
    width: 70px;
    height: 25px;
    line-height: 25px;
    margin: 5px 5px 0 5px;
    float: left;
    text-align: center;
    font-size: 15px;
    cursor: pointer;
}
</style>
```

- 未完待续...
