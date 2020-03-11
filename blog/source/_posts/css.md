---
title: CSS 常用集锦
index_img: /img/css.jpg
tags: [CSS]
abbrlink: f94fb1be
date: 2020-03-11 14:55:00
---


### 多行文本垂直居中
父元素 设置：display:table,height:500px;
子元素 设置：dispaly:table-cell;vertical-align:middle;

```html
<div class="parent">
<span class="child">
    <img src=""/>
    <span>垂直居中</span>
</span>
</div>
```
```css
.parent {
    display:table,height:500px
}
.child {
    dispaly:table-cell;vertical-align:middle
}

```

### webkit表单输入框placeholder的颜色值改变
```css
如果想要默认的颜色显示红色，代码如下：
input::-webkit-input-placeholder{color:red;}

如果想要用户点击变为蓝色，代码如下：
input:focus::-webkit-input-placeholder{color:blue;}
```

### 换行
word-break:break-all;只对英文起作用，以字母作为换行依据
word-wrap:break-word;只对英文起作用，以单词作为换行依据
white-space:pre-wrap;只对中文起作用，强制换行
white-space:nowrap;都起作用，强制不换行

white-space:nowrap;
overflow:hidden;
text-overflow:ellipsis;
width:500px;
不换行超出部分显示...

常用自定义类名：

```css
/*自动换行*/
.cut-text {
    word-break:break-all;
    word-wrap:break-word;
}
/*强制不换行*/
.no-cut-text {
    white-space:nowrap;
}
/*英文换行*/
.en-cut-text  {
    word-break:break-all;
}
/*超出显示...*/
.text-dots {
    width:500px;
    text-overflow:ellipsis;
    overflow:hidden;
    white-space:nowrap;
}
/*超出两行文本显示...*/ /*必须指定width*/
.text2LineRestrist {
    overflow: hidden;
    text-overflow:ellipsis;
    display:-webkit-box;
    -webkit-box-orient:vertical;
    -webkit-line-clamp:2;
}
/*超出三行文本显示...*/ /*必须指定width*/
.text3LineRestrist {
    overflow: hidden;
    text-overflow:ellipsis;
    display:-webkit-box;
    -webkit-box-orient:vertical;
    -webkit-line-clamp:3;
}
```


### 清除浮动
```css
.clearfix {zoom:1;}
.clearfix::after,.clearfix::before{
    content:"";
    display:block;
    clear: both;
    visibility: hidden;
    height:0;
}
```