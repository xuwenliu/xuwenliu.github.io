---
title: TS中的类
index_img: /img/ts/ts.jpg
tags: [TS]
category: 技术
abbrlink: f94fb1be
date: 2020-05-01 21:30:00
---

### 1.class的创建
```ts
class Person {
    name: string;
    age: number;
    constructor(name: string, age: number) { //构造方法 实例化类的时候触发的方法
        this.name = name;
        this.age = age;
    }
    run(): void {
        console.log(this.name + '在跑步');
    }
    getName(): string {
        return this.name;
    }
    setName(name: string): void {
        this.name = name;
    }
}

let p = new Person('张三', 30);
console.log(p.name); //张三
p.run();// 张三在跑步

console.log(p.getName());//张三
p.setName('李四');
console.log(p.getName());//李四
```

### 2.TS 类的继承 extends + super
```ts
class Person {
    name: string;
    age: number;
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    run(): void {
        console.log(this.name + '在跑步');
    }
    getName(): string {
        return this.name;
    }
    setName(name: string): void {
        this.name = name;
    }
}

//子类
class Student extends Person {
    sex: string;
    constructor(name: string, age: number, sex: string) {
        super(name, age); //初始化父类的构造函数
        this.sex = sex;
    }
    run(): void {
        console.log(this.name + '在跑步');
    }
    work(): void {
        console.log(this.name + '在工作');
    }
    print(): string {
        return this.name + '-' + this.age + '-' + this.sex;
    }
}

let s = new Student('李白', 400, '男');
console.log(s.sex); //男
s.run(); //李白在跑步
console.log(s.print()); //李白-400-男
```

### 3.类的修饰符 public(默认)  private   protected

| 是否可以访问 | 描述     | 父类 | 子类 | 类外部 |
| ------------ | -------- | ---- | ---- | ------ |
| public       | 公有     | ok   | ok   | ok     |
| private      | 私有     | ok   | no   | no     |
| protected    | 保护类型 | ok   | ok   | no     |



### 4.类的静态属性和静态方法
```ts
class Person {
    name: string;
    age: number;
    static sex:string = '男';
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    run(): void {
        console.log(this.name + '在跑步');
    }
    static print():void{ //静态方法中无法访问this(即不能访问实例属性和实例方法)
        console.log(Person.sex);
    }
}

let p = new Person('zs',56);
console.log(Person.sex); // 男
Person.print(); // 男
```

### 5.多态  
父类定义一个方法不去实现，让继承他的子类去实现，每个子类有不同的表现
多态属于继承
```ts
class Animal {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    eat(): void {
        console.log(this.name);
    }
}

class Dog extends Animal {
    constructor(name: string) {
        super(name);
    }
    eat():void{
        console.log(`${this.name} 要吃肉`);
    }
}

class Fish extends Animal {
    constructor(name:string){
        super(name);
    }
    eat():void {
        console.log(`${this.name} 要吃微生物`);
    }
}
```

### 6.抽象类  
+ 是提供其他类继承的基类，不能直接被实例化。
+ 用abstract关键字定义抽象类和抽象方法，抽象类中的抽象方法不包含具体实现，并且必须在派生类中实现。
+ 抽象方法只能放在抽象类中
+ 抽象类和抽象方法用来定义标准

```ts
abstract class Animal {
    name:string;
    constructor(name:string){
        this.name = name; 
    }
    abstract eat(): any; //抽象类中的抽象方法不包含具体实现
}

/* let a = new Animal(); //错误 不能直接被实例化 */
class Dog extends Animal {
    constructor(name:string){
        super(name);
    }
    // 抽象类的子类必须实现抽象类的抽象方法
    eat(): string {
        return this.name + '喜欢吃肉';
    }
}

let d = new Dog('小狗');
console.log(d.eat()); //小狗喜欢吃肉
```