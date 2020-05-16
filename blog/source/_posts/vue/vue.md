---
title: Vue 常用集锦
index_img: /img/vue.jpg
tags: [Vue]
category: 技术
abbrlink: f94fb1be
date: 2020-03-11 17:06:00
---

### 1.v-text/v-html 中如何使用过滤器
Vue2.x 不再支持在 `v-html` 中使用过滤器Vue2.x 的过滤器现在只能应用在“Mustache”语法 (双大括号) 的文本插值 和 `v-bind` 中。
如果要使用采用如下方式：

`v-html="$options.filters.过滤器名称(需要过滤的内容,[过滤器需要的参数])"`

### 2.在Vue组件中动态生成的DOM类名样式不作用-/deep/
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

### 3.使用a标签打电话
```html
<a :href="'tel:'+ phone">打电话</a>
```

### 4.注册自定义指令
+ 全局注册
    ```html
    <div id="app">
        <p>页面载入时，input 元素自动获取焦点：</p>
        <input v-focus>
    </div>
    
    <script>
    // 注册一个全局自定义指令 v-focus
    Vue.directive('focus', {
    // 当绑定元素插入到 DOM 中。
    inserted: function (el) {
        // 聚焦元素
        el.focus()
    }
    })
    // 创建根实例
    new Vue({
    el: '#app'
    })
    </script>
    ```

+ 局部注册

    ```html
    <div id="app">
    <p>页面载入时，input 元素自动获取焦点：</p>
    <input v-focus>
    </div>
    
    <script>
    // 创建根实例
    new Vue({
    el: '#app',
    directives: {
        // 注册一个局部的自定义指令 v-focus
        focus: {
        // 指令的定义
        inserted: function (el) {
            // 聚焦元素
            el.focus()
        }
        }
    }
    })
    </script>
    ```

### 5.Prop 验证
我们可以为组件的 prop 指定验证要求，例如你知道的这些类型。如果有一个需求没有被满足，则 Vue 会在浏览器控制台中警告你。这在开发一个会被别人用到的组件时尤其有帮助。

为了定制 prop 的验证方式，你可以为 props 中的值提供一个带有验证需求的`对象`，而不是一个`字符串数组`。例如：
```js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

### 6.子组件传数据到父组件

![子组件传数据到父组件](/img/vue/数据传递.png)


### 7.生命周期

![vue2.x生命周期](/img/vue/lifecycle.png)

### 8.生成二维码
[vue-qriously](https://github.com/theomessin/vue-qriously#readme)
[生成二维码并下载](https://segmentfault.com/a/1190000014875645)

+ 如何安装并导入
    ```js
    // ES6
    import Vue from 'vue'
    import VueQriously from 'vue-qriously'
    Vue.use(VueQriously)

    // ES5
    var Vue = require('vue')
    Vue.use(require('vue-qriously').default)

    ```

+ 如何使用
    ```html
    <qriously value="Hello World!" :size="200" />
    ```
参数：
+ background: string 二维码背景  默认：'white'
+ backgroundAlpha:number 二维码背景透明度 默认： 1.0
+ foreground:string 二维码前景色 默认：'black'
+ foregroundAlpha:number 二维码前景色透明度 默认：1.0
+ level:string 二维码误差校验等级(L,M,Q,H) 默认：'L'
+ mime:string 二维码图片的mine type
+ padding:number 二维码padding值(像素) 默认null(auto)
+ size:number 二维码大小(像素) 默认：100
+ value:string 二维码内容