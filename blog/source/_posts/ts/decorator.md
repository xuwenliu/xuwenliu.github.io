---
title: TS中的装饰器
index_img: /img/ts/ts.jpg
tags: [TS]
category: 技术
abbrlink: f94fb1be
date: 2020-05-05 22:53:00
---

### 1.什么是装饰器
装饰器是一种特殊类型的声明，它能够被附加到类，方法，属性或参数上，可以修改类的行为。
通俗的讲装饰器就是一个方法，可以注入到类，方法，属性参数上来拓展类，属性，方法，参数的功能。
常见的装饰器：
+ 类装饰器     
+ 属性装饰器   
+ 方法装饰器   
+ 参数装饰器

装饰器的写法：
+ 普通装饰器（无法传参）       
+ 装饰器工厂（可传参）

### 2.类装饰器

> 类装饰器----普通装饰器(无法传参)

```ts
function logClass(params: any) {
    console.log(params) //当前类

    //TODO ① 拓展-属性
    params.prototype.apiUrl = 'https://www.bilibili.com/video/av38379328/?p=19';
    
    // TODO ② 拓展-方法
    params.prototype.get = function(){
        console.log('我是拓展的方法' + this.apiUrl);
    }

}

@logClass
class HttpClient {
    constructor() {

    }
    getData() {

    }
}

let http: any = new HttpClient();

//TODO ① 访问拓展的属性
console.log(http.apiUrl); //https://www.bilibili.com/video/av38379328/?p=19

//TODO ② 访问拓展的方法
http.get(); // 我是拓展的方法https://www.bilibili.com/video/av38379328/?p=19
```

> 类装饰器----装饰器工厂(可传参)---返回一个函数

```ts
function logClass(params: string) {
    return function (target: any) {
        console.log(params) // 传入的参数 https://www.bilibili.com/video/av38379328/?p=19
        console.log(target) // 当前类

        // 拓展 apiUrl 属性，其值为传入的 params;
        target.prototype.apiUrl = params;
    }
}

@logClass('https://www.bilibili.com/video/av38379328/?p=19')
class HttpClient {
    constructor() {

    }
    getData() {

    }
}

let http: any = new HttpClient();
console.log('使用装饰器工厂拓展的属性', http.apiUrl); //使用装饰器工厂拓展的属性 https://www.bilibili.com/video/av38379328/?p=19
```

> 类装饰器-重载构造函数的例子---返回一个继承于当前类的子类

```ts
// 类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数。
// 如果类装饰器返回一个值，它会使用提供的构造函数来替换类的声明。

function logClass(target: any) {
    console.log(target) // target 当前类
    return class extends target {
        // TODO ① 重载apiUrl的值
        apiUrl = '我是修改后的数据  https://www.bilibili.com/video/av38379328/?p=19';

        // TODO ② HttpClient类里面的方法被重载了
        getData() {
            this.apiUrl += '---';
            console.log(this.apiUrl)
        }
    }
}

@logClass
class HttpClient {
    public apiUrl: string | undefined;
    constructor() {
        // TODO ① 初始化apiUrl的值
        this.apiUrl = "我是constructor里面的apiUrl";
    }

    // TODO ② HttpClient类里面的方法
    getData() {
        console.log(this.apiUrl);
    }
}

let http: any = new HttpClient();
http.getData(); // 我是修改后的数据  https://www.bilibili.com/video/av38379328/?p=19---
```

### 3.属性装饰器
属性装饰器表达式会在运行时当作函数被调用，传入2个参数
1.对于静态成员来说是类的构造函数，对于实例成员来说是类的原型对象。
2.成员的名称。

```ts
// 1.实例成员
function logProperty(params: string) {
    return function (target: any, attrName: string) {
        console.log(params); // http://www.baidu.com

        console.log(target); // TODO ① HttpClient 这个类的原型对象（即:HttpClient.prototype）  HttpClient { getData: [Function] }
        console.log(attrName) //apiUrl

        target[attrName] = params;
    }
}
class HttpClient {
    @logProperty('http://www.baidu.com')
    public apiUrl: string | undefined; //TODO ① 这里声明为实例属性

    constructor() {
    }

    getData() {
        console.log(this.apiUrl);
    }
}

let http: any = new HttpClient();
http.getData(); // http://www.baidu.com
```

```ts
//2.静态成员
function logProperty(params: string) {
    return function (target: any, attrName: string) {
        console.log(params); // http://www.baidu.com/2

        console.log(target);// TODO ② HttpClient 这个类  [Function: HttpClient]
        console.log(attrName) //apiUrl2

        target[attrName] = params; //由于是静态属性 这里就直接把params 挂到类上
    }
}
class HttpClient {

    @logProperty('http://www.baidu.com/2')
    static apiUrl2: string | undefined; //TODO ② 这里声明为静态成员

    constructor() {
    }

    getData() {
        console.log(HttpClient.apiUrl2); // 静态属性通过 HttpClient.属性名 来访问
    }
}

let http: any = new HttpClient();
http.getData(); // http://www.baidu.com/2
```

### 4.方法装饰器

它会被应用到方法的 属性描述符上，可以用来监视，修改或者替换方法定义。
方法装饰器表达式会在运行时当作函数被调用，传入3个参数
1.对于静态成员来说是类的构造函数，对于实例成员来说是类的原型对象。
2.成员的名称。
3.成员的属性描述符。

```ts
//这里只演示：实例成员
function logFunc(params: string) {
    return function (target: any, funcName: string, desc: any) {
        console.log(params); // http://www.jd.com
        console.log(target); // HttpClient.prototype
        console.log(funcName); // getData
        console.log(desc);
        /*
            { 
                value: [Function],
                writable: true,
                enumerable: true,
                configurable: true 
            }
        */

        // 修改装饰的方法（操作desc.value） 把装饰器方法里面传入的参数全部修改为string类型。
        // 1.保存当前方法
        let oldFunc = desc.value;
        // 2.重写 desc.value
        desc.value = function (...args: any[]) {
            console.log(args); // [123, '456']

            // 3.转换为String
            args = args.map(value => {
                return String(value);
            })
            console.log(args); // [ '123', '456' ]

            // 4.选择替换该方法还是给方法增加功能
            oldFunc.apply(this, args); // 加上这个就会执行HttpClient 类里面的getData 方法。
            // 不加 就是把【HttpClient 类里面的getData 方法】给替换了。
        }
    }
}

class HttpClient {
    apiUrl: string | undefined;
    constructor() {
    }
    @logFunc('http://www.jd.com')
    getData(...args: any[]) {
        console.log(args); // 经过装饰器的修改，这里全部被转为了String类型 [ '123', '456' ]
        console.log('我是getData');
    }
}

let http: any = new HttpClient();
http.getData(123, '456');
```

### 5.方法参数装饰器（不常用）
参数装饰器表达式会在运行时当作函数被调用，可以使用参数装饰器为类的原型增加一些元数据，传入3个参数
1.对于静态成员来说是类的构造函数，对于实例成员来说是类的原型对象。
2.方法的名称。
3.参数在函数参数列表中的索引。

```ts
function logParams(params: string) {
    return function (target: any, funcName: string, paramsIndex: number) {
        console.log(params) // name         id
        console.log(target) // HttpClient.prototype
        console.log(funcName) // getData
        console.log(paramsIndex) // 1       0
    }
}

class HttpClient {
    url: any | undefined;
    constructor() {
    }
    getData(
        @logParams('id') id: number, //后执行
        @logParams('name') name: string // 先执行
    ) {

    }
}
```

### 6.执行顺序

> 属性装饰器--->方法装饰器--->方法参数装饰器--->类装饰器
> 如果有多个同样的装饰器都是 从后到前执行