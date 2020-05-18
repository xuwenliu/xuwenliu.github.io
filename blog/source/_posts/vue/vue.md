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

### 16.图片懒加载
```js
import VueLazyload from 'vue-lazyload'
Vue.use(VueLazyload, {
    error: require('./assets/img/game-icon/yxdt_icon_none.png'),
    loading: require('./assets/img/loading.gif'),
    attempt: 1
})
```
使用：
```html
<ul>
  <li v-for="img in list">
    <img v-lazy="img.src" >
  </li>
</ul>
```

### 17.vue里面弹框滑动阻止body滚动
1.main.js
```js
//解决遮罩层滚动穿透问题，分别在遮罩层弹出后和关闭前调用
const ModalHelper = ( (bodyCls) =>{
    let scrollTop;
    return {
        open: function () {
            scrollTop = document.scrollingElement.scrollTop;
            document.body.classList.add(bodyCls);
            document.body.style.top = -scrollTop + 'px';
        },
        close: function () {
            document.body.classList.remove(bodyCls);
            // scrollTop lost after set position:fixed, restore it back.
            document.scrollingElement.scrollTop = scrollTop;
        }
    };
})('dialog-open');
Vue.prototype.ModalHelper = ModalHelper
```
2.body添加class `dialog-open`
```css
body.dialog-open {
    position: fixed;
    width: 100%;
}
```
3.监控 是否显示弹框变量
```js
watch:{
    isShow(newVal,oldVal){
        if(newVal){
            this.ModalHelper.open();
        }else {
            this.ModalHelper.close();
        }
    }
}
```

### 18.抠出mint-ui Loadmore源码自定义
```html
<template>
    <div class="mint-loadmore">
        <div class="mint-loadmore-content" :class="{ 'is-dropped': topDropped || bottomDropped}" :style="{ 'transform': 'translate3d(0, ' + translate + 'px, 0)' }">
            <slot name="top">
                <div class="mint-loadmore-top" v-if="topMethod">
                    <mt-spinner v-if="topStatus === 'loading'" class="mint-loadmore-spinner" :size="size" type="fading-circle"></mt-spinner>
                    <span class="mint-loadmore-text">{{ topText }}</span>
                </div>
            </slot>
            <slot></slot>
            <slot name="bottom">
                <div class="mint-loadmore-bottom"  v-if="bottomMethod">
                    <mt-spinner v-if="bottomStatus === 'loading'" class="mint-loadmore-spinner" :size="size" type="fading-circle"></mt-spinner>
                    <span class="mint-loadmore-text">{{ bottomText }}</span>
                </div>
            </slot>
    
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            stopTranslate: {
                type: Number,
                default: 80
            },
            maxDistance: {
                type: Number,
                default: 0
            },
            autoFill: {
                type: Boolean,
                default: false
            },
            distanceIndex: {
                type: Number,
                default: 2
            },
            topPullText: {
                type: String,
                default: '下拉刷新'
            },
            topDropText: {
                type: String,
                default: '释放刷新'
            },
            topLoadingText: {
                type: String,
                default: '刷新中...'
            },
            topDistance: {
                type: Number,
                default: 70
            },
            topMethod: {
                type: Function
            },
            bottomPullText: {
                type: String,
                default: '上拉加载'
            },
            bottomDropText: {
                type: String,
                default: '释放加载'
            },
            bottomLoadingText: {
                type: String,
                default: '疯狂加载中...'
            },
            bottomDistance: {
                type: Number,
                default: 70
            },
            bottomMethod: {
                type: Function
            },
            bottomAllLoaded: {
                type: Boolean,
                default: false
            }
        },
        data() {
            return {
                size: parseInt(this.HTML_FONT_SIZE * 0.4),
                translate: 0, // 此变量决定当前组件上下移动,
                scrollEventTarget: null, // 滚动的dom节点
                containerFilled: false, // 当前滚动的内容是否填充完整,不完成会调用 loadmore的回调函数
                topText: "", // 下拉刷新,显示的文本
                topDropped: false, // 记录当前drop状态,用给组件dom添加is-dropped class(添加回到原点的动画)
                bottomText: "", // 上拉加载更多 显示的文本
                bottomDropped: false, // 同topDropped
                bottomReached: false, // 当前滚动是否滚动到了底部
                direction: "", // touch-move过程中, 当前滑动的方向
                startY: 0, // touch-start 起始的y的坐标值
                startScrollTop: 0, // touch-start 起始的滚动dom的 scrollTop
                currentY: 0, // touch-move 过程中的 y的坐标
                topStatus: "", // 下拉刷新的状态: pull(下拉) drop(释放) loading(正在加载数据)
                bottomStatus: "" // 上拉加载更多的状态: 状态同上
            };
        },
        watch: {
            topStatus(val) {
                this.$emit("top-status-change", val);
                switch (val) {
                    case "pull":
                        this.topText = this.topPullText;
                        break;
                    case "drop":
                        this.topText = this.topDropText;
                        break;
                    case "loading":
                        this.topText = this.topLoadingText;
                        break;
                }
            },
    
            bottomStatus(val) {
                this.$emit("bottom-status-change", val);
                switch (val) {
                    case "pull":
                        this.bottomText = this.bottomPullText;
                        break;
                    case "drop":
                        this.bottomText = this.bottomDropText;
                        break;
                    case "loading":
                        this.bottomText = this.bottomLoadingText;
                        break;
                }
            }
        },
        mounted() {
            this.init(); // 当前 vue component挂载完成之后, 执行init()函数
        },
        methods: {
            onTopLoaded: function onTopLoaded() {
                var this$1 = this;
    
                this.translate = 0;
                setTimeout(function() {
                    this$1.topStatus = 'pull';
                }, 200);
            },
            onBottomLoaded: function onBottomLoaded() {
                var this$1 = this;
    
                this.bottomStatus = 'pull';
                this.bottomDropped = false;
                this.$nextTick(function() {
                    if (this$1.scrollEventTarget === window) {
                        document.body.scrollTop += this.stopTranslate;;
                    } else {
                        this$1.scrollEventTarget.scrollTop += this.stopTranslate;;
                    }
                    this$1.translate = 0;
                });
                if (!this.bottomAllLoaded && !this.containerFilled) {
                    this.fillContainer();
                }
            },
            init() {
                this.topStatus = 'pull';
                this.bottomStatus = 'pull';
                this.topText = this.topPullText;
                this.scrollEventTarget = this.getScrollEventTarget(this.$el); // 获取滚动的dom节点
                if (typeof this.bottomMethod === 'function') {
                    this.fillContainer(); // 判断当前滚动内容是否填满,没有执行外部传入的loadmore回调函数加载数据
                    this.bindTouchEvents(); // 为当前组件dom注册touch事件
                }
                if (typeof this.topMethod === 'function') {
                    this.bindTouchEvents();
                }
            },
            getScrollEventTarget: function getScrollEventTarget(element) {
                var currentNode = element;
                while (currentNode && currentNode.tagName !== 'HTML' &&
                    currentNode.tagName !== 'BODY' && currentNode.nodeType === 1) {
                    var overflowY = document.defaultView.getComputedStyle(currentNode).overflowY;
                    if (overflowY === 'scroll' || overflowY === 'auto') {
                        return currentNode;
                    }
                    currentNode = currentNode.parentNode;
                }
                return window;
            },
            getScrollTop: function getScrollTop(element) {
                if (element === window) {
                    return Math.max(window.pageYOffset || 0, document.documentElement.scrollTop);
                } else {
                    return element.scrollTop;
                }
            },
            bindTouchEvents: function bindTouchEvents() {
                this.$el.addEventListener('touchstart', this.handleTouchStart);
                this.$el.addEventListener('touchmove', this.handleTouchMove);
                this.$el.addEventListener('touchend', this.handleTouchEnd);
            },
    
    
            fillContainer() {
                if (this.autoFill) {
                    this.$nextTick(() => {
                        if (this.scrollEventTarget === window) {
                            this.containerFilled = this.$el.getBoundingClientRect().bottom >=
                                document.documentElement.getBoundingClientRect().bottom;
                        } else {
                            this.containerFilled = this.$el.getBoundingClientRect().bottom >=
                                this.scrollEventTarget.getBoundingClientRect().bottom;
                        }
                        if (!this.containerFilled) { // 如果没有填满内容, 执行loadmore的操作
                            this.bottomStatus = 'loading';
                            this.bottomMethod(); // 调用外部的loadmore函数,加载更多数据
                        }
                    });
                }
            },
            handleTouchStart(event) {
                this.startY = event.touches[0].clientY; // 手指按下的位置, 用于下面move事件计算手指移动的距离
                this.startScrollTop = this.getScrollTop(this.scrollEventTarget); // 起始scroll dom的 scrollTop(滚动的距离)
                //下面重置状态变量
                this.bottomReached = false;
                if (this.topStatus !== 'loading') {
                    this.topStatus = 'pull';
                    this.topDropped = false;
                }
                if (this.bottomStatus !== 'loading') {
                    this.bottomStatus = 'pull';
                    this.bottomDropped = false;
                }
            },
            handleTouchMove(event) {
                //确保当前touch节点的y的位置,在当前loadmore组件的内部
                if (this.startY < this.$el.getBoundingClientRect().top && this.startY > this.$el.getBoundingClientRect().bottom) {
                    return;
                }
                this.currentY = event.touches[0].clientY;
                let distance = (this.currentY - this.startY) / this.distanceIndex;
                this.direction = distance > 0 ? 'down' : 'up';
                // 下拉刷新,条件(1.外部传入了刷新的回调函数 2.滑动方向是向下的 3.当前滚动节点的scrollTop为0 4.当前topStatus不是loading)
                if (typeof this.topMethod === 'function' && this.direction === 'down' &&
                    this.getScrollTop(this.scrollEventTarget) === 0 && this.topStatus !== 'loading') {
                    event.preventDefault();
                    event.stopPropagation();
                    //计算translate(将要平移的距离), 如果当前移动的距离大于设置的最大距离,那么此次这次移动就不起作用了
                    if (this.maxDistance > 0) {
                        this.translate = distance <= this.maxDistance ? distance - this.startScrollTop : this.translate;
                    } else {
                        this.translate = distance - this.startScrollTop;
                    }
                    if (this.translate < 0) {
                        this.translate = 0;
                    }
                    this.topStatus = this.translate >= this.topDistance ? 'drop' : 'pull'; // drop: 到达指定的阈值,可以执行刷新操作了
                }
    
                // 上拉操作, 判断当前scroll dom是否滚动到了底部
                if (this.direction === 'up') {
                    this.bottomReached = this.bottomReached || this.checkBottomReached();
                }
                if (typeof this.bottomMethod === 'function' && this.direction === 'up' &&
                    this.bottomReached && this.bottomStatus !== 'loading' && !this.bottomAllLoaded) {
                    event.preventDefault();
                    event.stopPropagation();
                    // 判断的逻辑思路同上
                    if (this.maxDistance > 0) {
                        this.translate = Math.abs(distance) <= this.maxDistance ?
                            this.getScrollTop(this.scrollEventTarget) - this.startScrollTop + distance : this.translate;
                    } else {
                        this.translate = this.getScrollTop(this.scrollEventTarget) - this.startScrollTop + distance;
                    }
                    if (this.translate > 0) {
                        this.translate = 0;
                    }
                    this.bottomStatus = -this.translate >= this.bottomDistance ? 'drop' : 'pull';
                }
                this.$emit('translate-change', this.translate);
            },
            checkBottomReached() {
                if (this.scrollEventTarget === window) {
                    return document.body.scrollTop + document.documentElement.clientHeight >= document.body.scrollHeight;
                } else {
                    return this.$el.getBoundingClientRect().bottom <= this.scrollEventTarget.getBoundingClientRect().bottom + 1;
                }
            },
            handleTouchEnd() {
                if (this.direction === 'down' && this.getScrollTop(this.scrollEventTarget) === 0 && this.translate > 0) {
                    this.topDropped = true; // 为当前组件添加 is-dropped class(也就是添加动画处理)
                    if (this.topStatus === 'drop') { // 到达了loading的状态
                        this.translate = this.stopTranslate; // top slot的高度
                        this.topStatus = 'loading';
                        this.topMethod(); // 执行回调函数
                    } else { // 没有到达,回调原点
                        this.translate = '0';
                        this.topStatus = 'pull';
                    }
                }
                // 处理逻辑同上
                if (this.direction === 'up' && this.bottomReached && this.translate < 0) {
                    this.bottomDropped = true;
                    this.bottomReached = false;
                    if (this.bottomStatus === 'drop') {
                        this.translate = -this.stopTranslate;
                        this.bottomStatus = 'loading';
                        this.bottomMethod();
                    } else {
                        this.translate = '0';
                        this.bottomStatus = 'pull';
                    }
                }
                this.$emit('translate-change', this.translate);
                this.direction = '';
            }
        }
    
    
    
    
    
    }
</script>

<style lang="less" scoped>
    @import url('../components/less/common.less');
    .mint-spinner-fading-circle {
        position: relative
    }
    
    .mint-spinner-fading-circle-circle {
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        position: absolute
    }
    
    .mint-spinner-fading-circle-circle::before {
        content: " ";
        display: block;
        margin: 0 auto;
        width: 15%;
        height: 15%;
        border-radius: 100%;
        -webkit-animation: mint-fading-circle 1.2s infinite ease-in-out both;
        animation: mint-fading-circle 1.2s infinite ease-in-out both
    }
    
    .mint-spinner-fading-circle-circle.is-circle2 {
        -webkit-transform: rotate(30deg);
        transform: rotate(30deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle2::before {
        -webkit-animation-delay: -1.1s;
        animation-delay: -1.1s
    }
    
    .mint-spinner-fading-circle-circle.is-circle3 {
        -webkit-transform: rotate(60deg);
        transform: rotate(60deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle3::before {
        -webkit-animation-delay: -1s;
        animation-delay: -1s
    }
    
    .mint-spinner-fading-circle-circle.is-circle4 {
        -webkit-transform: rotate(90deg);
        transform: rotate(90deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle4::before {
        -webkit-animation-delay: -0.9s;
        animation-delay: -0.9s
    }
    
    .mint-spinner-fading-circle-circle.is-circle5 {
        -webkit-transform: rotate(120deg);
        transform: rotate(120deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle5::before {
        -webkit-animation-delay: -0.8s;
        animation-delay: -0.8s
    }
    
    .mint-spinner-fading-circle-circle.is-circle6 {
        -webkit-transform: rotate(150deg);
        transform: rotate(150deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle6::before {
        -webkit-animation-delay: -0.7s;
        animation-delay: -0.7s
    }
    
    .mint-spinner-fading-circle-circle.is-circle7 {
        -webkit-transform: rotate(180deg);
        transform: rotate(180deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle7::before {
        -webkit-animation-delay: -0.6s;
        animation-delay: -0.6s
    }
    
    .mint-spinner-fading-circle-circle.is-circle8 {
        -webkit-transform: rotate(210deg);
        transform: rotate(210deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle8::before {
        -webkit-animation-delay: -0.5s;
        animation-delay: -0.5s
    }
    
    .mint-spinner-fading-circle-circle.is-circle9 {
        -webkit-transform: rotate(240deg);
        transform: rotate(240deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle9::before {
        -webkit-animation-delay: -0.4s;
        animation-delay: -0.4s
    }
    
    .mint-spinner-fading-circle-circle.is-circle10 {
        -webkit-transform: rotate(270deg);
        transform: rotate(270deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle10::before {
        -webkit-animation-delay: -0.3s;
        animation-delay: -0.3s
    }
    
    .mint-spinner-fading-circle-circle.is-circle11 {
        -webkit-transform: rotate(300deg);
        transform: rotate(300deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle11::before {
        -webkit-animation-delay: -0.2s;
        animation-delay: -0.2s
    }
    
    .mint-spinner-fading-circle-circle.is-circle12 {
        -webkit-transform: rotate(330deg);
        transform: rotate(330deg)
    }
    
    .mint-spinner-fading-circle-circle.is-circle12::before {
        -webkit-animation-delay: -0.1s;
        animation-delay: -0.1s
    }
    
    @-webkit-keyframes mint-fading-circle {
        0%,
        39%,
        100% {
            opacity: 0
        }
        40% {
            opacity: 1
        }
    }
    
    @keyframes mint-fading-circle {
        0%,
        39%,
        100% {
            opacity: 0
        }
        40% {
            opacity: 1
        }
    }
    
    .mint-loadmore {
        overflow: hidden
    }
    
    .mint-loadmore-content {}
    
    .mint-loadmore-content.is-dropped {
        -webkit-transition: .2s;
        transition: .2s
    }
    
    .mint-loadmore-top,
    .mint-loadmore-bottom {
        text-align: center;
        height: 50px;
        line-height: 50px
    }
    
    .mint-loadmore-top {
        margin-top: -1.6rem /* 120/75 */;
        height: 1.6rem /* 120/75 */;
        line-height: 1.6rem /* 120/75 */;
        span {
            display: inline-block;
            vertical-align: middle;
            &.is-rotate {
                -webkit-transform: rotate(180deg);
                transform: rotate(180deg)
            }
        }
        /deep/.mint-spinner-fading-circle{
            display: block;
        }
    }
    
    .mint-loadmore-bottom {
        margin-bottom: -1.6rem /* 120/75 */;
        height: 1.6rem /* 120/75 */;
        line-height: 1.6rem /* 120/75 */;
    }
    
    .mint-loadmore-spinner {
        display: inline-block;
        margin-right: 5px;
        vertical-align: middle
    }
    
    .mint-loadmore-text {
        vertical-align: middle;
        font-size: .32rem/* 24/75 */;
        color: @color-green;
    }
.mint-loadmore-bottom{
    margin-bottom: -1.6rem /* 120/75 */;
    height: 1.6rem /* 120/75 */;
    line-height: 1.6rem /* 120/75 */;
        span {
        display: inline-block;
        -webkit-transition: .2s linear;
        transition: .2s linear;
        vertical-align: middle;
        &.is-rotate {
            -webkit-transform: rotate(180deg);
            transform: rotate(180deg)
        }
    }
    /deep/.mint-spinner-fading-circle{
        display: block;
    }
}
</style>
```

使用：
```html
<template>
    <div>
        <Header rooter="-1" title="自助返水" :hasNoBack="true" iFontsize=".58667rem"></Header>
        <!-- 查看返水详情弹框 -->
        <div v-show="isShowBackwaterMask" class="copy-writer-details-mask" @touchmove.self.prevent></div>
        <div v-show="isShowBackwaterMask" class="copy-writer-details-box">
            <div class="details-title pk-1px-b">
                <span>返水详情</span>
                <span @click="isShowBackwaterMask = false;"><i class="iconfont icon-sykszz-close"></i> </span>
            </div>
            <div class="details-content">
                <dl>
                    <dt class="pk-1px-b">
                            <span>游戏名称</span>
                            <span>有效打码</span>
                            <span>返水金额</span>
                        </dt>
                    <div class="scorll-list">
                        <dd v-for="(item,index) in infoData.listList" :key="index" class="pk-1px-b">
                            <span>{{item.platformname}}</span>
                            <span>{{item.betall}}</span>
                            <span>{{item.money}}</span>
                        </dd>
                    </div>
    
                </dl>
            </div>
        </div>
    
    
        <div class="content">
            <div class="content-fixed" v-show="infoData.betall>0">
                <div class="backwater-top">
                    <ul>
                        <li>
                            <h2>有效打码</h2>
                            <p v-show="!isShowBackWaterMoney">****</p>
                            <p v-show="isShowBackWaterMoney">{{infoData.betall}}</p>
                        </li>
                        <li>
                            <h2>返水金额</h2>
                            <p v-show="!isShowBackWaterMoney">****</p>
                            <p v-show="isShowBackWaterMoney">{{infoData.allmoney}}</p>
                        </li>
                    </ul>
                    <div class="backwater-top-btn">
                        <div v-show="isShowBackWaterMoney" class="look" @click="isShowBackwaterMask = true">点击查看返水详情</div>
                        <div class="btn">
                            <button @click="getBackWaterInfo()">查看返水额</button>
                            <button :disabled="infoData.status === 2 || !isShowBackWaterMoney" @click="handleBackWater">领取返水</button>
                        </div>
    
                    </div>
                </div>
                <div v-show="list.length>0" class="backwater-list">
                    <div class="title">自助返水历史</div>
                    <div class="total pk-1px-b">
                        <p>当日已返水：<span>{{today}}</span></p>
                        <p>本周返水额：<span>{{week}}</span></p>
                    </div>
                    <dl>
                        <dt class="pk-1px-b">
                                <span>有效打码</span>
                                <span>返水金额</span>
                            </dt>
                    </dl>
                </div>
            </div>
    
            <div class="page-loadmore">
                <div class="page-loadmore-wrapper" ref="wrapper" :style="{ height: wrapperHeight + 'px' }">
                    <pk-loadmore :top-method="loadTop" :bottom-method="loadBottom" :bottom-all-loaded="allLoaded" @top-status-change="handleTopChange" @bottom-status-change="handleBottomChange" ref="loadmore" :stop-translate="stopTranslate">
                        <ul>
                            <li v-for="(item,index) in list" :key="index" class="pk-1px-b">
                                <h2>
                                    <span>{{item.betall}}</span>
                                    <span>{{item.rebatewater}}</span>
                                </h2>
                                <p>
                                    <span>订单号：{{item.orderid}}</span>
                                    <span>{{item.createtime | filterDate}}</span>
                                </p>
                            </li>
                        </ul>
                        <div class="nodata" v-show="hasData">我是有底线的</div>
                    </pk-loadmore>
                </div>
            </div>
    
            <div v-show="infoData.betall<=0" class="no-data">
                <div class="no-data-box">
                    <i class="iconfont icon-list-zanwusj"></i>
                    <p>您暂时还未获得返水哦~~</p>
                    <button>去投注，获得返水！</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import pkLoadmore from '../../../components/Loadmore'
    import Header from '../../../components/Header'
    const backwater = proto.pb;
    export default {
        name: 'backwater',
        components: {
            Header,
            pkLoadmore
        },
        mounted() {
            this.getBackWaterInfo(1);
            this.getList();
        },
        watch: {
            isShowBackwaterMask(newVal, oldVal) {
                if (newVal) {
                    this.ModalHelper.open();
                } else {
                    this.ModalHelper.close();
                }
            }
        },
        data() {
            return {
                isShowBackwaterMask: false,
    
                allLoaded: false,
                topStatus: '',
                bottomStatus: '',
                wrapperHeight: 0,
                stopTranslate: parseInt(this.HTML_FONT_SIZE * 1.6),
                hasData: false,
    
                page: 1, //当前页数
                pageSize: 10, //每页请求的条数
                totalNum: 0, //总页数
                list: [],
                infoData: {},
                today: 0,
                week: 0,
    
                isShowBackWaterMoney: false,
    
            }
        },
        methods: {
            //获取返水列表
            getList(t) {
                let posts = {
                    page: this.page,
                    pageSize: this.pageSize,
                }
                let pageData = new backwater.PageParams();
                pageData.setPage(posts.page);
                pageData.setPagesize(posts.pageSize);
    
                let postData = new backwater.BackWaterReq();
                postData.setPageparams(pageData);
                let datas = postData.serializeBinary();
    
                this.ajax("PURSE_BACK_WATER_LIST", backwater.BackWaterResp, datas).then(res => {
                    this.today = res.today;
                    this.week = res.week;
                    this.totalNum = res.totalnum;
                    let result = res.listList;
                    if (this.page === 1) {
                        this.list = result;
                    } else {
                        this.list = this.list.concat(result);
                    }
                    if (t === 1) {
                        this.$toast({
                            message: '刷新成功',
                            duration: 2000
                        })
                    }
                    this.$nextTick(() => {
                        this.wrapperHeight = document.documentElement.clientHeight - this.$refs.wrapper.getBoundingClientRect().top;
                    })
                }).catch(err =>{})
            },
            //获取返水金额以及是否能返水
            getBackWaterInfo(t) {
                this.ajax("PURSE_BACK_WATER_INFO", backwater.BackWaterInfoResp).then(res => {
                    this.infoData = res;
                    this.isShowBackWaterMoney = !t;
                }).catch(err =>{})
            },
            //领取返水
            handleBackWater() {
                this.ajax("PURSE_BACK_WATER_RECEIVE", null).then(res => {
                    this.$toast({
                        message: '领取成功',
                        duration: 2000
                    });
                    this.getBackWaterInfo();
                }).catch(err =>{})
            },
            //下拉刷新
            handleTopChange(status) {
                this.topStatus = status;
            },
            loadTop() {
                this.page = 1;
                this.hasData = false;
                setTimeout(() => {
                    this.getList(1);
                    this.$refs.loadmore.onTopLoaded();
                    this.allLoaded = false;
                }, 1500);
            },
            //mint-ui 上拉加载
            handleBottomChange(status) {
                this.bottomStatus = status;
            },
            loadBottom() {
                this.page += 1;
                this.hasData = false;
                setTimeout(() => {
                    this.getList();
                    this.$refs.loadmore.onBottomLoaded();
                    if (this.page * this.pageSize >= this.totalNum) {
                        this.allLoaded = true; //所有数据加载完成
                        this.hasData = true;
                    }
                }, 1500);
            },
        }
    
    }
</script>

<style lang="less" scoped>
    @import url('../../../components/less/common.less');
    .no-data {
        padding: 0 .4rem/* 30/75 */;
       .no-data-box {
            margin-top: 2.13333rem/* 160/75 */;
            text-align: center;
            i{
                font-size: 2.53333rem /* 190/75 */;
                color: @color-8976cc;
                opacity: .6;
            }
            p {
                text-align: center;
                font-size: .42667rem/* 32/75 */
                ;
                color: @color-8976cc;
                margin-top: .26667rem/* 20/75 */
                ;
            }
            button {
                width: 100%;
                height: 1.06667rem/* 80/75 */
                ;
                line-height: 1.06667rem/* 80/75 */
                ;
                color: #fff;
                text-align: center;
                border: none;
                border-radius: .13333rem/* 10/75 */
                ;
                font-size: .37333rem/* 28/75 */
                ;
                background: @color-green;
                box-shadow: 0px 2px 5px 0px rgba(0, 0, 0, 0.12);
                margin-top: .53333rem/* 40/75 */
                ;
                &:active {
                    background: @color-00cc8f;
                }
            }
            button.look {
                margin-top: .4rem/* 30/75 */
                ;
                background: #fff;
                border: 1px solid @color-green;
                color: @color-green;
            }
        }
    }
    
    .content {
        .content-fixed {
            position: fixed;
            width: 100%;
            top: 1.22667rem/* 92/75 */
            ;
            left: 0;
            right: 0;
        }
        .backwater-top {
            position: relative;
            ul {
                background: @color-252232;
                display: flex;
                justify-content: space-around;
                text-align: center;
                height: 3.41333rem/* 256/75 */
                ;
                padding-top: .74667rem/* 56/75 */
                ;
                li {
                    flex: 1;
                    display: flex;
                    flex-direction: column;
                    h2 {
                        font-size: .4rem/* 30/75 */
                        ;
                        color: #fff;
                        font-weight: normal;
                    }
                    p {
                        margin-top: .44rem/* 33/75 */
                        ;
                        font-size: .48rem/* 36/75 */
                        ;
                        color: @color-green;
                    }
                }
            }
            .backwater-top-btn {
                position: absolute;
                bottom: -.53333rem/* 40/75 */
                ;
                width: 100%;
                display: flex;
                flex-direction: column;
                justify-content: space-around;
                .look {
                    font-size: .37333rem/* 28/75 */
                    ;
                    color: @color-8976cc;
                    text-align: center;
                    margin-bottom: .4rem/* 30/75 */
                    ;
                }
                .btn {
                    display: flex;
                    justify-content: space-around;
                    button {
                        width: 3.2rem/* 240/75 */
                        ;
                        height: 1.06667rem/* 80/75 */
                        ;
                        ;
                        line-height: 1.06667rem/* 80/75 */
                        ;
                        font-size: .37333rem/* 28/75 */
                        ;
                        border-radius: .13333rem/* 10/75 */
                        ;
                        border: none;
                        background: @color-green;
                        color: @color-252232;
                        box-shadow: 0px 2px 5px 0px rgba(0, 0, 0, 0.12);
                        &:last-child {
                            color: #fff;
                        }
                        &:disabled {
                            background: @color-add9cc;
                            box-shadow: none;
                            color: @color-c8c8cc;
                        }
                    }
                }
            }
        }
        .backwater-list {
            margin-top: .53333rem/* 40/75 */
            ;
            .title {
                line-height: 1.06667rem/* 80/75 */
                ;
                padding-left: .4rem/* 30/75 */
                ;
                font-size: .42667rem/* 32/75 */
                ;
                color: @color-323233;
            }
            .total {
                background: #fff;
                padding: 0 .4rem/* 30/75 */
                ;
                height: 1.06667rem/* 80/75 */
                ;
                display: flex;
                justify-content: space-between;
                align-items: center;
                p {
                    font-size: .37333rem/* 28/75 */
                    ;
                    color: @color-646466;
                    span {
                        color: @color-green;
                        font-weight: bold;
                    }
                }
            }
            dl {
                background: #fff;
                padding: 0 .4rem/* 30/75 */
                ;
                dt {
                    padding: .33333rem/* 25/75 */
                    0;
                    display: flex;
                    justify-content: space-between;
                    font-size: .42667rem/* 32/75 */
                    ;
                    color: @color-323233;
                    font-weight: bold;
                }
            }
        }
        .page-loadmore {
            position: fixed;
            top: 9.14667rem/* 686/75 */
            ;
            left: 0;
            right: 0;
            ul {
                background: #fff;
                padding: 0 .4rem/* 30/75 */
                ;
                li {
                    padding: .33333rem/* 25/75 */
                    0;
                    h2 {
                        display: flex;
                        justify-content: space-between;
                        color: @color-323233;
                        font-size: .37333rem/* 28/75 */
                        ;
                        font-weight: bold;
                    }
                    p {
                        display: flex;
                        justify-content: space-between;
                        color: @color-969699;
                        margin-top: .26667rem/* 20/75 */
                        ;
                        font-size: .32rem/* 24/75 */
                        ;
                    }
                }
            }
        }
    }
</style>
```

### 19.抠出mint-ui datePicker 源码
```html
<template>
    <mt-popup v-model="visible" :closeOnClickModal="closeOnClickModal" position="bottom" class="mint-datetime">
        <div class="popup-title pk-1px-b">
            <span @click="visible = false;$emit('cancel')">{{cancelText}}</span>
            <span>{{titleText}}</span>
            <span @click="confirm">{{confirmText}}</span>
        </div>
        <mt-picker :itemHeight="itemHeight" :slots="dateSlots" @change="onChange" :visible-item-count="visibleItemCount" class="mint-datetime-picker" ref="picker">
        </mt-picker>
    </mt-popup>
</template>

<style lang="less">
    @import url("./less/common.less");
    .popup-title {
        height: 1.06667rem/* 80/75 */
        ;
        padding: 0 0.4rem/* 30/75 */
        ;
        font-size: 0.4rem/* 30/75 */
        ;
        color: @color-323233;
        text-align: center;
        display: flex;
        justify-content: space-between;
        align-items: center;
        span {
            flex: 1;
            height: 1.06667rem/* 80/75 */
            ;
            line-height: 1.06667rem/* 80/75 */
            ;
        }
        span:first-child {
            color: @color-323233;
            text-align: left;
        }
        span:last-child {
            color: @color-green;
            text-align: right;
        }
    }
</style>

<script>
    const FORMAT_MAP = {
        Y: "year",
        M: "month",
        D: "date",
        H: "hour",
        m: "minute"
    };
    
    export default {
        name: "mt-datetime-picker",
        props: {
            titleText: {
                type: String,
                default: "请选择时间"
            },
            cancelText: {
                type: String,
                default: "取消"
            },
            confirmText: {
                type: String,
                default: "确定"
            },
            type: {
                type: String,
                default: "datetime"
            },
            startDate: {
                type: Date,
                default () {
                    return new Date(new Date().getFullYear() - 10, 0, 1);
                }
            },
            endDate: {
                type: Date,
                default () {
                    return new Date(new Date().getFullYear() + 10, 11, 31);
                }
            },
            startHour: {
                type: Number,
                default: 0
            },
            endHour: {
                type: Number,
                default: 23
            },
            yearFormat: {
                type: String,
                default: "{value}"
            },
            monthFormat: {
                type: String,
                default: "{value}"
            },
            dateFormat: {
                type: String,
                default: "{value}"
            },
            hourFormat: {
                type: String,
                default: "{value}"
            },
            minuteFormat: {
                type: String,
                default: "{value}"
            },
            visibleItemCount: {
                type: Number,
                default: 5
            },
            closeOnClickModal: {
                type: Boolean,
                default: true
            },
            value: null
        },
    
        data() {
            return {
                visible: false,
                startYear: null,
                endYear: null,
                startMonth: 1,
                endMonth: 12,
                startDay: 1,
                endDay: 31,
                currentValue: null,
                selfTriggered: false,
                dateSlots: [],
                shortMonthDates: [],
                longMonthDates: [],
                febDates: [],
                leapFebDates: [],
                itemHeight: 36
            };
        },
        methods: {
            open() {
                this.visible = true;
            },
    
            close() {
                this.visible = false;
            },
    
            isLeapYear(year) {
                return year % 400 === 0 || (year % 100 !== 0 && year % 4 === 0);
            },
    
            isShortMonth(month) {
                return [4, 6, 9, 11].indexOf(month) > -1;
            },
    
            getMonthEndDay(year, month) {
                if (this.isShortMonth(month)) {
                    return 30;
                } else if (month === 2) {
                    return this.isLeapYear(year) ? 29 : 28;
                } else {
                    return 31;
                }
            },
    
            getTrueValue(formattedValue) {
                if (!formattedValue) return;
                while (isNaN(parseInt(formattedValue, 10))) {
                    formattedValue = formattedValue.slice(1);
                }
                return parseInt(formattedValue, 10);
            },
    
            getValue(values) {
                let value;
                if (this.type === "time") {
                    value = values
                        .map(value => ("0" + this.getTrueValue(value)).slice(-2))
                        .join(":");
                } else {
                    let year = this.getTrueValue(values[0]);
                    let month = this.getTrueValue(values[1]);
                    let date = this.getTrueValue(values[2]);
                    let maxDate = this.getMonthEndDay(year, month);
                    if (date > maxDate) {
                        this.selfTriggered = true;
                        date = 1;
                    }
                    let hour =
                        this.typeStr.indexOf("H") > -1 ?
                        this.getTrueValue(values[this.typeStr.indexOf("H")]) :
                        0;
                    let minute =
                        this.typeStr.indexOf("m") > -1 ?
                        this.getTrueValue(values[this.typeStr.indexOf("m")]) :
                        0;
                    value = new Date(year, month - 1, date, hour, minute);
                }
                return value;
            },
    
            onChange(picker) {
                let values = picker.$children
                    .filter(child => child.currentValue !== undefined)
                    .map(child => child.currentValue);
                if (this.selfTriggered) {
                    this.selfTriggered = false;
                    return;
                }
                if (values.length !== 0) {
                    this.currentValue = this.getValue(values);
                    this.handleValueChange();
                }
            },
    
            fillValues(type, start, end) {
                let values = [];
                for (let i = start; i <= end; i++) {
                    if (i < 10) {
                        values.push(
                            this[`${FORMAT_MAP[type]}Format`].replace(
                                "{value}",
                                ("0" + i).slice(-2)
                            )
                        );
                    } else {
                        values.push(this[`${FORMAT_MAP[type]}Format`].replace("{value}", i));
                    }
                }
                return values;
            },
    
            pushSlots(slots, type, start, end) {
                slots.push({
                    flex: 1,
                    values: this.fillValues(type, start, end)
                });
            },
    
            generateSlots() {
                let dateSlots = [];
                const INTERVAL_MAP = {
                    Y: this.rims.year,
                    M: this.rims.month,
                    D: this.rims.date,
                    H: this.rims.hour,
                    m: this.rims.min
                };
                let typesArr = this.typeStr.split("");
                typesArr.forEach(type => {
                    if (INTERVAL_MAP[type]) {
                        this.pushSlots.apply(
                            null, [dateSlots, type].concat(INTERVAL_MAP[type])
                        );
                    }
                });
                if (this.typeStr === "Hm") {
                    dateSlots.splice(1, 0, {
                        divider: true,
                        content: ":"
                    });
                }
                this.dateSlots = dateSlots;
                this.handleExceededValue();
            },
    
            handleExceededValue() {
                let values = [];
                if (this.type === "time") {
                    const currentValue = this.currentValue.split(":");
                    values = [
                        this.hourFormat.replace("{value}", currentValue[0]),
                        this.minuteFormat.replace("{value}", currentValue[1])
                    ];
                } else {
                    values = [
                        this.yearFormat.replace("{value}", this.getYear(this.currentValue)),
                        this.monthFormat.replace(
                            "{value}",
                            ("0" + this.getMonth(this.currentValue)).slice(-2)
                        ),
                        this.dateFormat.replace(
                            "{value}",
                            ("0" + this.getDate(this.currentValue)).slice(-2)
                        )
                    ];
                    if (this.type === "datetime") {
                        values.push(
                            this.hourFormat.replace(
                                "{value}",
                                ("0" + this.getHour(this.currentValue)).slice(-2)
                            ),
                            this.minuteFormat.replace(
                                "{value}",
                                ("0" + this.getMinute(this.currentValue)).slice(-2)
                            )
                        );
                    }
                }
                this.dateSlots
                    .filter(child => child.values !== undefined)
                    .map(slot => slot.values)
                    .forEach((slotValues, index) => {
                        if (slotValues.indexOf(values[index]) === -1) {
                            values[index] = slotValues[0];
                        }
                    });
                this.$nextTick(() => {
                    this.setSlotsByValues(values);
                });
            },
    
            setSlotsByValues(values) {
                const setSlotValue = this.$refs.picker.setSlotValue;
                if (this.type === "time") {
                    setSlotValue(0, values[0]);
                    setSlotValue(1, values[1]);
                }
                if (this.type !== "time") {
                    setSlotValue(0, values[0]);
                    setSlotValue(1, values[1]);
                    setSlotValue(2, values[2]);
                    if (this.type === "datetime") {
                        setSlotValue(3, values[3]);
                        setSlotValue(4, values[4]);
                    }
                }
                [].forEach.call(this.$refs.picker.$children, child =>
                    child.doOnValueChange()
                );
            },
    
            rimDetect(result, rim) {
                let position = rim === "start" ? 0 : 1;
                let rimDate = rim === "start" ? this.startDate : this.endDate;
                if (this.getYear(this.currentValue) === rimDate.getFullYear()) {
                    result.month[position] = rimDate.getMonth() + 1;
                    if (this.getMonth(this.currentValue) === rimDate.getMonth() + 1) {
                        result.date[position] = rimDate.getDate();
                        if (this.getDate(this.currentValue) === rimDate.getDate()) {
                            result.hour[position] = rimDate.getHours();
                            if (this.getHour(this.currentValue) === rimDate.getHours()) {
                                result.min[position] = rimDate.getMinutes();
                            }
                        }
                    }
                }
            },
    
            isDateString(str) {
                return /\d{4}(\-|\/|.)\d{1,2}\1\d{1,2}/.test(str);
            },
    
            getYear(value) {
                return this.isDateString(value) ?
                    value.split(" ")[0].split(/-|\/|\./)[0] :
                    value.getFullYear();
            },
    
            getMonth(value) {
                return this.isDateString(value) ?
                    value.split(" ")[0].split(/-|\/|\./)[1] :
                    value.getMonth() + 1;
            },
    
            getDate(value) {
                return this.isDateString(value) ?
                    value.split(" ")[0].split(/-|\/|\./)[2] :
                    value.getDate();
            },
    
            getHour(value) {
                if (this.isDateString(value)) {
                    const str = value.split(" ")[1] || "00:00:00";
                    return str.split(":")[0];
                }
                return value.getHours();
            },
    
            getMinute(value) {
                if (this.isDateString(value)) {
                    const str = value.split(" ")[1] || "00:00:00";
                    return str.split(":")[1];
                }
                return value.getMinutes();
            },
    
            confirm() {
                this.visible = false;
                this.$emit("confirm", this.currentValue);
            },
    
            handleValueChange() {
                this.$emit("input", this.currentValue);
            }
        },
    
        computed: {
            rims() {
                if (!this.currentValue)
                    return {
                        year: [],
                        month: [],
                        date: [],
                        hour: [],
                        min: []
                    };
                let result;
                if (this.type === "time") {
                    result = {
                        hour: [this.startHour, this.endHour],
                        min: [0, 59]
                    };
                    return result;
                }
                result = {
                    year: [this.startDate.getFullYear(), this.endDate.getFullYear()],
                    month: [1, 12],
                    date: [
                        1,
                        this.getMonthEndDay(
                            this.getYear(this.currentValue),
                            this.getMonth(this.currentValue)
                        )
                    ],
                    hour: [0, 23],
                    min: [0, 59]
                };
                this.rimDetect(result, "start");
                this.rimDetect(result, "end");
                return result;
            },
    
            typeStr() {
                if (this.type === "time") {
                    return "Hm";
                } else if (this.type === "date") {
                    return "YMD";
                } else {
                    return "YMDHm";
                }
            }
        },
    
        watch: {
            value(val) {
                this.currentValue = val;
            },
    
            rims() {
                this.generateSlots();
            },
    
            visible(val) {
                this.$emit("visible-change", val);
            }
        },
        mounted() {
            this.itemHeight = parseInt(this.HTML_FONT_SIZE * 1.06667);//新增的
            this.currentValue = this.value;
            if (!this.value) {
                if (this.type.indexOf("date") > -1) {
                    this.currentValue = this.startDate;
                } else {
                    this.currentValue = `${("0" + this.startHour).slice(-2)}:00`;
                }
            }
            this.generateSlots();
        }
    };
</script>
```