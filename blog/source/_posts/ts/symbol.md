---
title: TS中的Symbol
index_img: /img/ts/ts.jpg
tags: [TS]
category: 技术
abbrlink: f94fb1be
date: 2020-05-08 20:26:00
---

### 1.ES6新增基本数据类型

> 自ECMAScript 2015起，`symbol`成为了一种新的原生类型，表示独一无二的值。就像`number`和`string`一样。它是 JavaScript 语言的第七种数据类型，前六种是：`undefined`、`null`、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。`symbol`类型的值是通过`Symbol`构造函数创建的。



```ts
let sym1 = Symbol();
let sym2 = Symbol("key"); // 可选的字符串key
```

注意，`Symbol`函数前不能使用`new`命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。



如何访问Symbol的描述，如上及如何访问到 “key”

[ES2019](https://github.com/tc39/proposal-Symbol-description) 提供了一个实例属性`description`，直接返回 Symbol 的描述。

```ts
const sym = Symbol('key');
sym.description // "key"
```



### 2.Symbol是不可改变且唯一的

```ts
// 无参数的情况
let sym2 = Symbol();
let sym3 = Symbol();

sym2 === sym3; // false, symbol是唯一的

// 有参数的情况
let s1 = Symbol('foo');
let s2 = Symbol('foo');

s1 === s2 // false, symbol是唯一的
```



### 3.Symbol 值不能与其他类型的值进行运算

```ts
let sym = Symbol('My symbol');

"your symbol is " + sym   // TypeError: can't convert symbol to string
`your symbol is ${sym}`   // TypeError: can't convert symbol to string
```

但是，Symbol 值可以显式转为字符串。

```ts
let sym = Symbol('My symbol');

String(sym) // 'Symbol(My symbol)'
sym.toString() // 'Symbol(My symbol)'
```

Symbol 值也可以转为布尔值，但是不能转为数值。

```ts
let sym = Symbol();
Boolean(sym) // true
!sym  // false

if (sym) {
  // ...
}

Number(sym) // TypeError
sym + 2 // TypeError
```



### 4.Symbol可以被用做对象属性的键

```ts
let sym1 = Symbol();
let obj = {
    [sym1]: "value"
};

console.log(obj[sym1]); // "value"
```



### 5.Symbol可以与计算出的属性名声明相结合来声明对象的属性和类成员

```ts
const sym = Symbol();

class Abc {
    [sym](){
       return "ABC";
    }
}

let c = new Abc();
let className = c[sym](); // "ABC"
```



### 6.内置的 Symbol 值

参考：[https://es6.ruanyifeng.com/#docs/symbol#%E5%86%85%E7%BD%AE%E7%9A%84-Symbol-%E5%80%BC](https://es6.ruanyifeng.com/#docs/symbol#内置的-Symbol-值)



