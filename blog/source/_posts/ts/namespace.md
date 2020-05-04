---
title: TS中的模块和命名空间
index_img: /img/ts/ts.jpg
tags: [TS]
category: 技术
abbrlink: f94fb1be
date: 2020-05-03 22:52:00
---

### 1.模块的概念
我们可以把一些公共的功能单独抽离成一个文件作为模块。
模块里面的变量，函数，类等默认是私有的，如果我们要在外部访问模块里面的（变量，函数，类），我们需要通过export 暴露出模块里面的（变量，函数，类）。暴露后我们通过 import 引入模块中的（变量，函数，类），这样就可以使用了。


```ts
// db.ts
let dbUrl = 'xxxxx';
export function getData:any[]{
    console.log('获取数据');
	return [
        {
        	title:'123',
            name:'哈哈哈哈',
        }
    ]
}

//另一种暴露方式
export {
	dbUrl,
    getData
}

// export default 默认导出，每个模块都可以有一个default导出，并且每个模块只能有一个default导出。
export default getData;

//引入的时候 import getData from './db/; 没有{}了。
```

```ts
// index.ts 
import { getData } from './db';
getData();

// 别名
import { getData as get} from './db';
get();
```

### 2.命名空间和模块的区别
+ 命名空间：内部模块，主要用于组织代码，避免命名冲突。
+ 模块：ts的外部模块的简称，侧重代码的复用，一个模块里可以有多个命名空间。

```ts
// 暴露命名空间A
export namespace A{
	export function getData:any[]{
        console.log('获取数据');
        return [
            {
                title:'123',
                name:'哈哈哈哈',
            }
        ]
    }
}

// 暴露命名空间B
export namespace B{
	export function getData:any[]{
        console.log('获取数据');
        return [
            {
                title:'123',
                name:'哈哈哈哈',
            }
        ]
    }
}

// 内部使用：
A.getData();
B.getData();

// 外部使用 需要给namespace 也暴露出去
```