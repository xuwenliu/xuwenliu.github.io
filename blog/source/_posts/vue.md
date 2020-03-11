---
title: Vue 常用集锦
index_img: /img/vue.jpg
tags: [Vue]
abbrlink: f94fb1be
date: 2020-03-11 17:06:00
---

### v-text/v-html 中如何使用过滤器
Vue2.x 不再支持在 `v-html` 中使用过滤器Vue2.x 的过滤器现在只能应用在“Mustache”语法 (双大括号) 的文本插值 和 `v-bind` 中。
如果要使用采用如下方式：

`v-html="$options.filters.过滤器名称(需要过滤的内容,[过滤器需要的参数])"`

### 在Vue组件中动态生成的DOM类名样式不作用-/deep/
```html
<template>
    <div id="app">
        <div v-text="text"></div>
    </div>
</template>
<script>
    export default {
        name:"app",
        data(){
            return {
                text:<span class="red">红色</span>
            } 
        }
    }
</script>

<style scoped lang="less">
    /deep/ .red{
        color:red;
    }
</style>

```
注意：/deep/ 只是在最外层使用，不要嵌套使用。
参考文档：[deep](https://vue-loader.vuejs.org/guide/scoped-css.html)

