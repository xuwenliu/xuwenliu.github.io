---
title: TS中的泛型
index_img: /img/ts/ts.jpg
tags: [TS]
category: 技术
abbrlink: f94fb1be
date: 2020-05-04 21:59:00
---

### 1.泛型
软件工程中，我们不仅要创建一致的定义良好的API，同时也要考虑可重用性。
组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能。
通俗理解：泛型就是解决 类，接口，函数的复用性，以及对不特定的数据类型的支持。

### 2.泛型函数
```ts
function getData<T>(str: T): T {
    return str;
}

let str = getData<string>('aaa');
console.log(str);

let str2 = getData<number>(123);
console.log(str2);

let str3 = getData<number | string>("123");
console.log(str3);
```

### 3.泛型类

```ts
class MinClass<T> {
    list: Array<T> = [];
    add(num: T):void {
        this.list.push(num);
    }
    min(): T {
        let minVal = this.list[0];
        for (let i = 0; i < this.list.length; i++) {
            if (minVal > this.list[i]) {
                minVal = this.list[i];
            }
        }
        return minVal;
    }
}

let num = new MinClass<number>();
num.add(3);
num.add(1);
num.add(2);
console.log(num.min()) //1

let str = new MinClass<string>();
str.add('y');
str.add('x');
str.add('w');
console.log(str.min()) //w
```

### 4.泛型接口

```ts
// 泛型接口 写法1
interface IConfigFn {
    <T>(value: T): T;
}
let fs: IConfigFn = function <T>(value: T): T {
    return value;
}
let newFs = fs<number>(123);
console.log(newFs); // 123


// 泛型接口 写法2
interface IConfigFn2<T> {
    (value: T): T;
}
function getData<T>(vlaue: T): T {
    return vlaue;
}
let getMyData: IConfigFn2<boolean> = getData;
console.log(getMyData(false)); //false
```

```ts
interface IConfigFn {
    <T>(value: T): T;
}
let fs: IConfigFn = function <T>(value: T): T {
    return value;
}
console.log(fs<number>(123));



interface IConfigFn2<T> {
    (value: T): T;
}
function getData<T>(value: T): T {
    return value;
}
let fs2: IConfigFn2<number[]> = getData;
let fs3: IConfigFn2<Array<string>> = getData;
console.log(fs2([1, 2, 3]));
console.log(fs3(['a', 'b', 'c']));
```

### 5.把类作为参数类型的泛型类

```ts
// 把类作为参数来约束数据传入的类型
class User {
    username: string | undefined;
    password: string | undefined;
}

class ActicleCate {
    title: string | undefined;
    desc: string | undefined;
    constructor(params: {
        title: string,
        desc: string
    }) {
        this.title = params.title;
        this.desc = params.desc;
    }
}

class MySql<T>{
    add(value: T): boolean {
        console.log(value);
        return true;
    }
}

let db = new MySql<User>();
db.add({
    username: 'aaa',
    password: '123456'
});

let aDb = new MySql<ActicleCate>();
let arcticle = new ActicleCate({
    title: '今天好天气',
    desc: '25度，确实可以'
});

aDb.add(arcticle);
```
