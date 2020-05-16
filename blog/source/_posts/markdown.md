---
title: Markdown语法
index_img: /img/markdown.jpg
tags: [Markdown语法]
category: 技术
abbrlink: f94fb1be
date: 2020-05-15 20:10:00
---


## Markdown语法

### 1.标题

+ 使用 # 号标记 

```js
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```



效果如下：

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题



+ 使用 = 和 - 标记一级和二级标

```js
我展示的是一级标题
=================

我展示的是二级标题
-----------------
```



效果如下：

我展示的是一级标题
=================

我展示的是二级标题
-----------------



### 2.格式

+ 字体

    ```js
    *斜体文本*
    _斜体文本_
    
    **粗体文本**
    __粗体文本__
    
    ***粗斜体文本***
    ___粗斜体文本___
    ```

    

    效果如下：

    *斜体文本*
    _斜体文本_

    **粗体文本**
    __粗体文本__

    ***粗斜体文本***
    ___粗斜体文本___

    

+ 分割线

    你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线：

    ```js
    ***
    
    * * *
    
    *****
    
    - - -
    
    ----------
    ```

    

    效果如下：

    ***

    * * *

    *****

    - - -

    ----------

    

+ 删除线

    如果段落上的文字要添加删除线，只需要在文字的两端加上两个波浪线 **~~** 即可

    ```
    ~~删除线~~
    ```

    

    效果如下：

    ~~删除线~~

    

+ 下划线

    下划线可以通过 HTML 的 `<u>` 标签来实现：

    ```
    <u>带下划线文本</u>
    ```

    

    效果如下：

    <u>带下划线文本</u>

    

+ 脚注

    脚注是对文本的补充说明。

    ```js
    [^要注明的文本]
    ```

    

    ```
    创建脚注格式类似这样 [^百度]。
    
    [^百度]: 百度一下
    ```

    

    效果如下：

    创建脚注格式类似这样 [^百度]。

    

### 3.列表

+ 无序列表使用星号`*`、加号`+`或是减号`-`作为列表标记：

    ```js
    * 第一项
    * 第二项
    * 第三项
    
    + 第一项
    + 第二项
    + 第三项
    
    
    - 第一项
    - 第二项
    - 第三项
    ```

    

    效果如下：

    * 第一项
    * 第二项
    * 第三项

    + 第一项
    + 第二项
    + 第三项


    - 第一项

    - 第二项

    - 第三项

        

+ 有序列表使用数字并加上` . `号来表示，如：

    ```js
    1. 第一项
    2. 第二项
    3. 第三项
    ```

    

    效果如下：

    1. 第一项

    2. 第二项

    3. 第三项

        

+ 列表嵌套

    列表嵌套只需在子列表中的选项添加四个空格即可：

    ```js
    1. 第一项：
        - 第一项嵌套的第一个元素
        - 第一项嵌套的第二个元素
    2. 第二项：
        - 第二项嵌套的第一个元素
        - 第二项嵌套的第二个元素
    ```

    

    效果如下：

    1. 第一项：
        - 第一项嵌套的第一个元素
        - 第一项嵌套的第二个元素
    2. 第二项：
        - 第二项嵌套的第一个元素
        - 第二项嵌套的第二个元素

### 4.区块

Markdown 区块引用是在段落开头使用 **>** 符号 ，然后后面紧跟一个**空格**符号：

```js
> javascript
> python
> vue
```



效果如下：

> javascript
> python
> vue

另外区块是可以嵌套的，一个 **>** 符号是最外层，两个 **>** 符号是第一层嵌套，以此类推：

```
> 最外层
> > 第一层嵌套
> > > 第二层嵌套
```



效果如下：

> 最外层
> > 第一层嵌套
> >
> > > 第二层嵌套





列表和区块是可以相互组合使用的

### 5.代码

+ 如果是段落上的一个函数或片段的代码可以用反引号把它包起来（**`**），例如：

```
`printf()` 函数
```



效果如下：

`printf()` 函数



+ 用``` 包裹一段代码，并指定一种语言（也可以不指定）：

~~~js
```js
$(document).ready(function () {
    alert('1');
});
```
~~~



效果如下：

```js
$(document).ready(function () {
    alert('1');
});
```



### 6.链接

+ 链接使用方法如下：

```
[链接名称](链接地址)

或者

<链接地址>
```

例如：

```
这是一个链接 [百度一下](https://www.baidu.com)
```



效果如下：

这是一个链接 [百度一下](https://www.baidu.com)

+ 直接使用链接地址：

    ```
    <https://www.baidu.com>
    ```

    

    效果如下：

    <https://www.baidu.com>

+ 高级链接

    我们可以通过变量来设置一个链接，变量赋值在文档末尾进行：

    ```js
    这个链接用 1 作为网址变量 [百度][1]
    这个链接用 runoob 作为网址变量 [京东][jingdong]
    ```

    

    效果如下：

    这个链接用 1 作为网址变量 [百度][1]
    这个链接用 runoob 作为网址变量 [京东][jingdong]



### 7.图片

```
![alt 属性文本](图片地址 "可选标题")
```

- 开头一个感叹号 !
- 接着一个方括号，里面放上图片的替代文字
- 接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上选择性的 'title' 属性的文字。

```
![RUNOOB 图标](http://static.runoob.com/images/runoob-logo.png)

![RUNOOB 图标](http://static.runoob.com/images/runoob-logo.png "开开心心")
```



效果如下：

![RUNOOB 图标](http://static.runoob.com/images/runoob-logo.png)

![RUNOOB 图标](http://static.runoob.com/images/runoob-logo.png "开开心心")

#### 指定高度与宽度

Markdown 还没有办法指定图片的高度与宽度，如果你需要的话，你可以使用普通的 `<img> `标签。

```
<img src="http://static.runoob.com/images/runoob-logo.png" width="50px">
```



效果如下：

<img src="http://static.runoob.com/images/runoob-logo.png" width="50px">



### 8.表格

+ Markdown 制作表格使用 `| `来分隔不同的单元格，使用 `-` 来分隔表头和其他行。

```
|  表头   | 表头  |
|  ----  | ----  |
| 单元格  | 单元格 |
| 单元格  | 单元格 |
```



效果如下：

| 表头   | 表头   |
| ------ | ------ |
| 单元格 | 单元格 |
| 单元格 | 单元格 |

+ 对齐方式

    - **-:** 设置内容和标题栏居右对齐。

    - **:-** 设置内容和标题栏居左对齐。

    - **:-:** 设置内容和标题栏居中对齐。

        ```
        | 左对齐 | 右对齐 | 居中对齐 |
        | :-----| ----: | :----: |
        | 单元格 | 单元格 | 单元格 |
        | 单元格 | 单元格 | 单元格 |
        ```

        

        效果如下：

        | 左对齐 | 右对齐 | 居中对齐 |
        | :----- | -----: | :------: |
        | 单元格 | 单元格 |  单元格  |
        | 单元格 | 单元格 |  单元格  |

        

### 9.视频/音频

直接使用html标签 `video`和`audio`

```html
<video id="video" 
       controls="" 
       preload="none" 
       poster="http://img.blog.fandong.me/2017-08-26-Markdown-Advance-Video.jpg">
      <source id="mp4" 
              src="http://img.blog.fandong.me/2017-08-26-Markdown-Advance-Video.mp4" 		      type="video/mp4">
</video>
```

<video id="video" controls="" preload="none" poster="http://img.blog.fandong.me/2017-08-26-Markdown-Advance-Video.jpg">
      <source id="mp4" src="http://img.blog.fandong.me/2017-08-26-Markdown-Advance-Video.mp4" type="video/mp4">
      </video>





```html
<audio id="audio" controls="" preload="none">
      <source id="mp3" src="http://qiniu.cloud.fandong.me/Music_iP%E8%B5%B5%E9%9C%B2%20-%20%E7%A6%BB%E6%AD%8C%20%28Live%29.mp3">
      </audio>
```

<audio id="audio" controls="" preload="none">       <source id="mp3" src="http://qiniu.cloud.fandong.me/Music_iP%E8%B5%B5%E9%9C%B2%20-%20%E7%A6%BB%E6%AD%8C%20%28Live%29.mp3">       </audio>



### 10.支持的 HTML 元素

不在 Markdown 涵盖范围之内的标签，都可以直接在文档里面用 HTML 撰写。

目前支持的 HTML 元素有`<kbd>` `<b> ` `<i>` ` <em>` ` <sup> ` `<sub>` ` <br>`等 ，如：

```html
使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑
```

使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑



### 11.转义

Markdown 使用了很多特殊符号来表示特定的意义，如果需要显示特定的符号则需要使用转义字符，Markdown 使用反斜杠转义特殊字符：

```js
**文本加粗** 
\*\* 正常显示星号 \*\*
```



效果如下：

**文本加粗** 
\*\* 正常显示星号 \*\*



Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

```
\   反斜线
`   反引号
*   星号
_   下划线
{}  花括号
[]  方括号
()  小括号
#   井字号
+   加号
-   减号
.   英文句点
!   感叹号
```



### 12.公式和流程图、时序图(顺序图)、甘特图、逻辑图 这里不做介绍，平时用的少。要用就百度吧。



### 13.想在别的地方让别人给你的仓库Star的美好方式

src 属性中的 `https://ghbtns.com/github-btn.html` 是固定的

| 参数  | 必传 | 类型   | 备注     |
| :---- | :--- | :----- | :------- |
| user  | true | String | 用户名   |
| repo  | true | String | 仓库名字 |
| type  | true | String | star     |
| count | true | String | 数量     |

```html
<iframe
        style="margin-left: 2px; margin-bottom:-5px;"
        frameborder="0" scrolling="0" width="100px" height="20px"
        src="https://ghbtns.com/github-btn.html?user=xuwenliu&repo=music-cloud&type=star&count=true" ></iframe>

```

<iframe
        style="margin-left: 2px; margin-bottom:-5px;"
        frameborder="0" scrolling="0" width="100px" height="20px"
        src="https://ghbtns.com/github-btn.html?user=xuwenliu&repo=music-cloud&type=star&count=true" ></iframe>











[1]: http://www.baidu.com/
[Jingdong]: http://www.jd.com/

[^百度]: 百度一下