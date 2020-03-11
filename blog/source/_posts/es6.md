---
title: ES6/ES7 常用集锦
index_img: /img/es6.jpg
tags: [ES6,ES7]
abbrlink: f94fb1be
date: 2020-03-11 17:32:00
---

### Set
A.使用Set处理数组去重和删除元素，Set是es6新增的一种数据结构，它和数组非常相似，但是成员的值都是唯一的，没有重复的值。提供4个语义化的API：
1.`add(value)`;添加某个值，返回Set结构本身。
2.`delete(value)`;删除某个值，返回一个布尔值，表示删除是否成功。
3.`has(value)`;返回一个布尔值，表示改值是否为Set的成员。
4.`clear()`;清除所有成员，没有返回值。

```javascript
let arr= [1,2,3,4,4,5,5,5];
let set = new Set(arr);
console.log(...set); //1 2 3 4 5
```
注意：这个方法对元素为对象的数组不奏效。Set判断元素是否重复的方法类似于`===`。

B.删除数组中指定的元素：
```javascript
const arr = [1,2,3];//例如删除数组中值为2的元素
const index = arr.indexOf(2);
if(index != -1){
    arr.splice(index,1);
}
console.log(arr); //[1,3]

//使用set
const arr = [1,2,3];
const set = new Set(arr);
set.delete(2);
arr = [...set];
console.log(arr); //[1,3]
```

C.使用`map()`方法和对象解构语法提取字段
```javascript
let studentInfo = [
    { name: 'Alice', age: 18, no: 2 },
    { name: 'Bob', age: 16, no: 5 },
    { name: 'Candy', age: 17, no: 3 },
    { name: 'Den', age: 18, no: 4 },
    { name: 'Eve', age: 16, no: 1 },
]
const nameList = studentInfo.map(({name}) => name);
console.log(nameList); //["Alice", "Bob", "Candy", "Den", "Eve"]
```

D.使用`filter()`方法和对象解构语法过滤字段
```javascript
let studentInfo = [
    { name: 'Alice', age: 18, no: 2 },
    { name: 'Bob', age: 16, no: 5 },
    { name: 'Candy', age: 17, no: 3 },
    { name: 'Den', age: 18, no: 4 },
    { name: 'Eve', age: 16, no: 1 },
]
const newStudentInfo = studentInfo.filter(({age}) => age>17);
console.log(newStudentInfo); //[ { name: 'Alice', age: 18, no: 2 }, { name: 'Den', age: 18, no: 4 }]
```

E.数组遍历使用`for in`,`forEach`,`for of`
```javascript
let studentInfo = [
    { name: 'Alice', age: 18, no: 2 },
    { name: 'Bob', age: 16, no: 5 },
    { name: 'Candy', age: 17, no: 3 },
    { name: 'Den', age: 18, no: 4 },
    { name: 'Eve', age: 16, no: 1 },
]
for(let i in studentInfo) {
    console.log(i);//下标
    console.log(studentInfo[i]);
}
studentInfo.forEach((value,index,arr) => {
    console.log(value);
    console.log(index);
    console.log(arr);
})

for(let i of studentInfo) {
    console.log(i);//每一项的值 { name: 'Alice', age: 18, no: 2 }
}
```

F.最短的代码创建一个长度为6，每个值为8的数组
```javascript
Array(6).fill(8); //[8, 8, 8, 8, 8, 8]
```
更多详细参考：[阮一峰大神](http://es6.ruanyifeng.com/#docs/set-map)

### 解构
```javascript
//数组解构
let [a, b, c] = [4, 5, 6]; //a=4,b=5,c=6


//解构不成功其值为undefined
let [a] = []; //a = undefined
let [a, b] = [1];//b = undefined


//对象解构
let { a, b, c} = { b: "aaa", a: "bbb" };
//a = "bbb"
//b = "aaa"
//c = undefined


let { a : b } = { a: "aaa", c: "bbb" };
//b = "aaa"
//a  error: foo is not defined
//a是匹配的模式，b才是变量。真正被赋值的是变量baz，而不是模式foo。


//字符串解构
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"


//函数参数解构
function add([a, b]){
    return a + b;
}

add([1, 2]); // 3


常见的用途

// 1.交换变量的值
let x = 1;
let y = 2;
[x, y] = [y, x];

// 2.函数返回值

// 返回一个数组
function num() {
return [1, 2, 3];
}
let [a, b, c] = num();

// 返回一个对象
function example() {
    return {
        a: 1,
        b: 2
    };
}
let { a, b } = example();

// 3.解析json数据
let json = {
    id: 1,
    status: 404,
    data: [123, 456]
};
let { id, status, data: number } = json;
console.log(id, status, number);// 1, 404, [123, 456]
```

### String
1.模板字符串
2.JavaScript 只有`indexOf`方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6 又提供了三种新方法。
    1.`includes(target,[index])`：返回布尔值，表示是否找到了参数字符串。
    2.`startsWith(target,[index])`：返回布尔值，表示参数字符串是否在原字符串的头部。
    3.`endsWith(target,[index])`：返回布尔值，表示参数字符串是否在原字符串的尾部。
    `target` 目标字符串，`index`可选参数 开始搜索的位置

```javascript
let s = 'Hello world!';
s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true
s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```
3.`str.repeat(n)`方法 返回一个新字符串，表示将原字符串重复n次。 n可以是大于-1的数
```javascript
let s = "abc";
s.repeat(3); //abcabcabc 参数为NaN 等同于 0
s.repeat(-1) //RangeError 参数是负数或者Infinity，会报错。
s.repeat(1.9) //abcabc 参数如果是小数，会被取整。
s.repeat(-0.5) // "" 参数是 0 到-1 之间的小数，则等同于 0，这是因为会先进行取整运算。0 到-1 之间的小数，取整以后等于-0，repeat视同为 0。
```

### ES5的类
1.构造函数 实例属性和方法
 ```javascript
function Person() {
    this.name = "tom";
    this.age = 20;
    this.speak = function(){
        console.log(this.name + ':' + this.age);
    }
}

let p = new Person();
console.log(p.name); //tom
p.speak(); // tom:20
 ```