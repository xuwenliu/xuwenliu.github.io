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

### 9.60s短信验证码倒计时
countDown.vue
```html
<template>
     <div class="getInfobtn">
        <button>{{time | change}}</button>
     </div>
</template>

<script>
let flag = false
    export default {
        data () {
            return {
                time : '获取验证码',
            }
        },
        props : {
            start : {
                type : Boolean
            }
        },
        watch : {
            start (value,oldvalue) {
                if(value == true){
                    this.countDown();
                }
            }
        },
        created() {
            if(this.start==true){
                this.countDown();
            }
        },//加上这个可以实现点击另一个按钮触发这个组件的点击事件，对应的是：自动触发倒计时，就是默认就显示60s后重新获取
        methods: {
            countDown () {
                this.time = 60;
                let time = setInterval(()=>{
                    this.time --;
                    if(this.time == 0){
                        this.$emit('countDown');
                        this.time = '重新发送';
                        flag = true;
                        clearInterval(time);
                    }
                },1000)
            }
        },
        filters : {
            change (value) {
               if(!value) return "";
               if(!isNaN(value)){
                   if(flag == true){
                       return `${value}s后重新获取`;
                   }
                   return value+'s后重新获取';
               }else{
                   return value;
               }
            }
        }

    }
</script>
<style scoped>
.getInfobtn button{
    background-color: green;
    color:#fff;
    padding: 0.1rem;
    border-radius: 0.05rem;
    width: 3rem;
}
</style>
```
使用：
```html
<template>
	<div class="getSubsidy">
        //给组件绑定事件就加上.native
		<countDown :start='start' @countDown ='start=false' @click.native='sendCode'></countDown>
           <button @click="shout">点我触发上面的倒计时</button>
	</div>
</template>
<script>
	import countDown from "./countDown"
	export default{
		components:{
			countDown
		},
		data(){
			return {
				start : false
			}
		},
		methods:{
			sendCode (value) {
                //前面发送ajax请求成功之后调用this.start = true开始短信倒计时
                this.start = true
            },
            shout() {
          	    this.start = true;
            },//配合组件里面的created钩子函数自动倒计时。

		}
	}
</script>
```

### 10.在vue里面使用RSA加密
1.将jsencrypt.min.js复制到static文件夹下面
2.在index.html里面引入
```js
<script type=text/javascript src="./static/jsencrypt.min.js"></script>
```
3.定义加密函数
```js
<script>
    var Rsa = function(key) {
        var encrypt = new JSEncrypt(),
        publicKey = "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCBG3UFPAxh+a0NLv6Plvjo5YPDdnlbED8dI4GP21DdFKvXVFcPb0lSRrht5Xrg7ck4PJ/fovfSi7k8MYqPY52g9tnPzkAthVOs99Tw6DVe22vV2hcs7dXvtk+TxKy4IqMjZA77hiH8wMYcJur+o4R770mrVP4fP88x53EQ4PaayQIDAQAB";
        encrypt.setPublicKey(publicKey);
        return encrypt.encrypt(key);
    }
</script>
```
上面定义可以在methods里面定义
4.使用
```js
conosle.log(Rsa(123));
```

### 11.货币过滤器
```js
Vue.filter('currency', function (value, _currency, decimals) {
  var digitsRE = /(\d{3})(?=\d)/g;
  value = parseFloat(value);
  if (!isFinite(value) || !value && value !== 0) return '';
  _currency = _currency != null ? _currency : '$';
  decimals = decimals != null ? decimals : 2;
  var stringified = Math.abs(value).toFixed(decimals);
  var _int = decimals ? stringified.slice(0, -1 - decimals) : stringified;
  var i = _int.length % 3;
  var head = i > 0 ? _int.slice(0, i) + (_int.length > 3 ? ',' : '') : '';
  var _float = decimals ? stringified.slice(-1 - decimals) : '';
  var sign = value < 0 ? '-' : '';
  return sign + _currency + head + _int.slice(i).replace(digitsRE, '$1,') + _float;
});
```
### 12.时间过滤器
```js
export const filterDate = (date, fmt = 'YYYY-MM-DD HH:mm') => {
    if (!date) {
        return '';
    }
    if (typeof date === 'number') {
        date = new Date(date * 1000);
    }
    var o = {
      'M+': date.getMonth() + 1,
      'D+': date.getDate(),
      'h+': date.getHours() % 12 === 0 ? 12 : date.getHours() % 12,
      'H+': date.getHours(),
      'm+': date.getMinutes(),
      's+': date.getSeconds(),
      'q+': Math.floor((date.getMonth() + 3) / 3),
      'S': date.getMilliseconds()
    }
    var week = {
      '0': '\u65e5',
      '1': '\u4e00',
      '2': '\u4e8c',
      '3': '\u4e09',
      '4': '\u56db',
      '5': '\u4e94',
      '6': '\u516d'
    }
    if (/(Y+)/.test(fmt)) {
      fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length))
    }
    if (/(E+)/.test(fmt)) {
      fmt = fmt.replace(RegExp.$1, ((RegExp.$1.length > 1) ? (RegExp.$1.length > 2 ? '\u661f\u671f' : '\u5468') : '') + week[date.getDay() + ''])
    }
    for (var k in o) {
      if (new RegExp('(' + k + ')').test(fmt)) {
        fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? (o[k]) : (('00' + o[k]).substr(('' + o[k]).length)))
      }
    }
    return fmt
}
```

### 13.过滤电话号码182****1234
```js
Vue.filter('telfilter', function (value) {
  var data = value.split("");
  data.splice(3, 4, "*", "*", "*", "*");
  return data.join("");
})
```

### 14.vue-router 商品列表页进入商品详情页，返回商品列表回滚到之前走的位置
条件：
1.必须是body在滚动
2.商品详情页路由需要缓存
3.设置路由离开保存滚动高度，回到列表是设置之前保存的滚动高度。
```js
data:{
		return {
			scroll:0,//保存滚动高度
		}
	}，
beforeRouteLeave(to,from,next) {
		//离开的时候保存body的滚动高度
		this.scroll=document.body.scrollTop;
		next();
	},
beforeRouteEnter(to,from,next){
    //详情页回到列表页的时候设置body的滚动高度
    next(vm => {
        if(from.name=="getSubsidy"){//商品详情页的路由name
            document.body.scrollTop=vm.scroll;
        }
    })
},
```

### 15.vue里面获取滚动高度，并实现吸顶效果
1.布局
```html
<div class="xiding-outbox" ref="xid">
	<div class="xiding-innerbox" :class="{'xiding':isFixed}">
		<div class="help" @click="callHandleService(customerServicePhone)">
			<p class="F3 C0">
				<img src="../../../assets/img/material/customer_service_ic.png">
					如需帮助，点击咨询人工服务
				</p>
		</div>
		<div class="search">
			<div class="search-box">
				<div class="search-input">
					<input class="F4 C1" type="text" maxlength="30"
								placeholder="输入品牌名称搜索" v-model.trim="queryKey">
				</div>
				<a class="search-btn F4 C0" @click="search">搜索</a>
			</div>
			<button class="F4 C0 find" @click="isFade=!isFade">查找我要的品类
			<p></p>
		    </button>
		</div>
	</div>
</div>
```
2.样式
```css
.xiding-outbox{
    height: 3.68rem;//吸顶外面固定吸顶内容的高度
    .xiding-innerbox{
        .help{
            width: 100%;
            height: 1.78667rem;
            line-height: 1.78667rem;
            background-color: #282828;
            p{
                display: inline-block;
                position: absolute;
                left: 50%;
                margin-left: -3.04rem;
                img{
                    position: relative;
                    top: 0.2rem;
                    margin-right: 0.21333rem;
                }
            }
        }
        .search{
            background-color: #fff;
            padding: 0.50667rem 0.29333rem;
            border-bottom: 1px solid #e6e6e6;
            display: flex;
            justify-content:space-between;
            .search-box{
                    display: flex;
                .search-input{
                    border:1px solid #282828;
                    border-right: none;
                    width:3.4rem;
                    height: 0.8rem;
                    border-radius: 0.08rem 0 0 0.08rem;
                    overflow: hidden;
                    input{
                        display: block;
                        box-sizing: border-box;
                        width: 100%;
                        height: 0.8rem;
                        padding-left: 0.21333rem;
                        border-radius: 0.08rem 0 0 0.08rem;
                    }
                }
                .search-btn{
                    border-radius: 0 0.08rem 0.08rem 0;
                    border:1px solid #282828;
                    height: 0.8rem;
                    line-height: 0.8rem;
                    width: 1.2rem;
                    text-align: center;
                    background-color: #282828;
                }
                .search-btn:active {  
                    opacity: 0.5; 
                } 
            }
            
            .find{
                background-color: #ed316b;
                height: 0.85rem;
                width: 3.4rem;
                border-radius: 0.08rem;
                border:1px solid #ed316b;
                p{
                    display: inline-block;
                    width: 0.21333rem;
                    height: 0.21333rem;
                    border:1px solid ;
                    background: transparent;
                    border-color: #fff #fff transparent transparent;
                    transform:rotateZ(45deg);
                } 
            }
        }
    }
    .xiding{
        position: fixed;
        top: 1.2rem;
        left: 0;
        z-index: 99;
        width: 100%;
    }
}
```
3.在methods里面定义touchMove方法
```js
touchMove() {
    //获取滚动高度
    var bodyTop=window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
    //获取被吸顶元素的高度
    var xidH=this.$refs.xid.offsetTop;
    if(bodyTop>=xidH) {
        this.isFixed=true;
    }else{
        this.isFixed=false;

    }
},
```
4.在mounted钩子函数里面给window监听scroll事件
```js
mounted() {
    this.$nextTick(() => {
        window.addEventListener('scroll',this.touchMove);
    })
},
```