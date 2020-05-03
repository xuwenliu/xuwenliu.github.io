---
title: JavaScript 常用集锦
index_img: /img/js.jpg
tags: [JS]
category: 技术
abbrlink: f94fb1be
date: 2020-03-11 17:20:00
---

### 字符串基本操作
参考链接：[字符串操作](https://www.jb51.net/article/97915.htm)

### 数组基本操作
参考链接：[数组操作](http://www.cnblogs.com/blogs-8888/p/6518683.html)

### 字符串去空格
```javascript
let trimSpace = function(str,is_global){
    let result;
    result = str.replace(/(^\s+)|(\s+$)/g,"");
    if(is_global && is_global.toLowerCase()=="g"){
        result = result.replace(/\s/g,"");
    }
    return result;
}

```

### 获取url参数值
```javascript
function getQueryVal(url){
    let url = url || '';
    let arr = (url.split("?")[1] || url || '').split("&");
    let o={};
    if(arr[0]==''){return o}
    arr.forEach(function(x){
        let ar=x.split("=");
        o[ar[0]]=ar[1]
    });
    return o
}
eg: console.log(getQueryVal("http://192.168.28.50.9090/#/twitterInfo?isNeed=0&userType=c"));
//{isNeed: "0", userType: "c"}

```

### vue里面使用hash模式获取url参数值
```javascript
/**
 *  name [参数名称]
 */
let getHashString = function(name) {
    let url = window.location.hash.substr(1);
    let theRequest = {};
    if(url.indexOf("?")>-1){
        let  strs = url.split("?")[1].split("&");
        for(let i = 0; i < strs.length; i ++) {
            theRequest[strs[i].split("=")[0]] = unescape(strs[i].split("=")[1]);
        }
        for(let j in theRequest){
            if(name==j){
                return theRequest[j];
            }
        }  
    }else {
        return null;
    }
};
eg:  console.log(getHashString("userType"));
// c
```

### history模式获取url参数值
```javascript
/**
 * name [参数名称]
 */
let getQueryString = function(name) {
    let reg = new RegExp("(^|&)"+ name +"=([^&]*)(&|$)");
    let r = window.location.search.substr(1).match(reg);
    if(r!=null)return  unescape(r[2]); return null;
};
eg: 当前的url是http://localhost:4000/home/index.html?caseid=123&idx=1
console.log(getQueryString("caseid"));
// 123
```

### 函数节流
```javascript
function debounce(func,delay){
    let timer = null;
    return function(...args){
        if(timer){
            clearTimeout(timer);
        }
        timer = setTimeout(()=>{
            func.apply(this,args);
        },delay)        
    }
}

// 测试
function show(){
    console.log('hello...');
}

let func = debounce(show,3000);

// 调用
func(); func(); func(); func(); func(); 
// 连续调用时，没有超过三秒是不会有任何输出的
//hello...
```

### Async/Await是Generator和Promise的组合，完全解决了基于回调的异步
```javascript
let getKeyPromise = function () {
    return new Promsie(function (resolve, reject) {
        $.ajax({
            type: 'get',
            url: 'http://localhost:3000/apiKey',
            success: function (data) {
            let key = data;
            resolve(key);         
            },
            error: function (err) {
                reject(err);
            }
        });
    });
};

let getTokenPromise = function (key) {
    return new Promsie(function (resolve, reject) {
        $.ajax({
            type: 'get',
            url: 'http://localhost:3000/getToken',
            data: {
                key: key
            },
            success: function (data) {
                resolve(data);         
            },
            error: function (err) {
                reject(err);
            }
        });
    });
};

let getDataPromise = function (data) {
    let token = data.token;
    let userId = data.userId;
    
    return new Promsie(function (resolve, reject) {
        $.ajax({
            type: 'get',
            url: 'http://localhost:3000/getData',
            data: {
                token: token,
                userId: userId
            },
            success: function (data) {
                resolve(data);         
            },
            error: function (err) {
                reject(err);
            }
        });
    });
};

async function main () {
    let key = await getKeyPromise();
    let loginData = await getTokenPromise(key);
    let busiData = await getDataPromise(loginData);
    console.log('数据：', busiData);
}

main();
console.log('123');
```