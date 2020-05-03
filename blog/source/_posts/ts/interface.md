---
title: TS中的接口
index_img: /img/ts/interface.jpg
tags: [TS]
category: 技术
abbrlink: f94fb1be
date: 2020-05-02 22:47:00
---

接口的作用：定义标准
在面向对象的编程中，接口是一种规范的定义，它定义了行为和动作的规范，在程序设计里面，接口起到了一种限制和规范的作用。接口定义了某一批类所需要遵循的规范，接口不关心这些类的内部状态数据，也不关心这些类里方法的实现细节，它只规定这批类里必须提供某些方法，提供这些方法的类就可以满足实际需要。

### 1.属性接口     
> 对json的约束

```ts
interface IObj {
    name: string;
    age?: number;
}
function getList(obj: IObj): void {
    console.log(obj.name);
    console.log(obj.age);
}
let obj = {
    name: 'tom',
    age: 22,//可选属性，可传，可不传
}
getList(obj);
```


```ts
interface IObj {
    name: string;
    age: number;
}

function getList(obj: IObj): void {
    console.log(obj.name);
    console.log(obj.age);
}
//定义在外面 只要包含name，age就可以
let obj = {
    name: 'tom',
    age: 20,
    firstName: 't', //额外的属性
}
getList(obj);

//定义在里面 只能有name，age
getList({
    name: 'tom',
    age: 20,
    firstName: 't', //错误
});

//正确
getList({
    name: 'tom',
    age: 20,
});

```

### 2.函数接口  

> 对函数传入的参数和返回值进行约束
```ts
interface encrypt {
    (key: string, value: string): string;
}

let md5: encrypt = function (key: string, value: string): string {
    return key + '--md5-' + value;
};

let sha1: encrypt = function (key: string, value: string): string {
    return key + '--sha1-' + value;
};
console.log(md5('name', '数学'));
console.log(sha1('name', '历史'));
```

### 3.可索引接口   
> 对数组、对象的约束（不常用）

```ts
//对 数组的约束
interface userArray {
    [index: number]: string;
}
let arr: userArray = ['a', '123'];


// 对 对象的约束
interface userObj {
    [index: string]: string | number;
}
let obj: userObj = {
    name: '张三',
}

// 对 数组对象的约束
interface userArrayObj {
    [index: number]: userObj
}
let list: userArrayObj = [{
    name: '张三',
    age: 20
}];
```

### 4.类 类型接口   
> 对类的约束 和抽象类有点相似

```ts
// 类 类型接口  implements 实现接口
interface IAnimal {
    name: string;
    age: number;
    eat(str: string): void;
    run(value: number): string;
}

class Dogs implements IAnimal {
    name: string;
    age: number;
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    eat(str: string): void {
        console.log(str);
    }
    run(value: number): string {
        return '我跑了' + value;
    }
}
```

### 5.接口扩展（接口可以继承接口）

```ts
interface IAnimal {
    name: string;
    age: number;
    eat(str: string): void;
    run(value: number): string;
}

interface IPerson extends IAnimal {
    cate: string;
    print(str: string): void;
}

class Person implements IPerson {

    name: string;
    age: number;
    cate: string;
    constructor(name: string, age: number, cate: string) {
        this.name = name;
        this.age = age;
        this.cate = cate;
    }

    print(str: string): void {
        console.log(str);
    }
    eat(str: string): void {
        console.log(str + '吃饭')
    }
    run(value: number): string {
        return '我跑了' + value;
    }
}

let s = new Person('小小', 20, '人');
console.log(s.run(60));
s.eat('小小');
```

```ts
interface IAnimal {
    name: string;
    age: number;
    eat(str: string): void;
    run(value: number): string;
}

interface IPerson extends IAnimal {
    cate: string;
    print(str: string): void;
}

class Person implements IPerson {

    name: string;
    age: number;
    cate: string;
    constructor(name: string, age: number, cate: string) {
        this.name = name;
        this.age = age;
        this.cate = cate;
    }

    print(str: string): void {
        console.log(str);
    }
    eat(str: string): void {
        console.log(str + '吃饭')
    }
    run(value: number): string {
        return '我跑了' + value;
    }
}


// 同时继承类 和 实现接口
class Programmer extends Person implements IPerson {
    constructor(name: string, age: number, cate: string) {
        super(name, age, cate);
    }

    print(str: string): void {
        console.log(str);
    }
    eat(str: string): void {
        console.log(str + '吃饭')
    }
    run(value: number): string {
        return '我跑了' + value;
    }
}

let s = new Person('小小', 20, '人');
console.log(s.run(60));
s.eat('小小');
```