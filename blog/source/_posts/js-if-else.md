---
title: JS 处理如何优雅的处理复杂判断 
index_img: /img/js.jpg
tags: [JS]
category: 技术
abbrlink: f94fb1be
date: 2020-04-29 20:10:00
---

### 1.基本if else 逻辑判断
```js
let sex = 1;
let sexText = '';
if(sex === 1){
  sexText = '男';
}else if(sex === 2){
  sexText = '女';
}else {
  sexText = '其他';
}
```
### 2.switch/case
```js
const getStatusText = (status) => {
 let str = '';
  switch (status) {
    case 1:
      str="待开通";
      break;
    case 2:
      str="开通准备中";
      break;
    case 3:
      str="开通准备完成";
      break;
    default:
      str="已过期";
      break;
 }
  return str;
}
```
### 3.三元表达式
```js
let sex = 1;
let sexText = sex === 1 ? '男' : sex === 2 ? '女': '其他';
```
### 4.短路 && 与 ||
&& 中第一个表达式为假就不会去处理第二个表达式，|| 则相反

```js
// if为真
if (bool) {
 value = getValueTrue();
}
// &&改版
bool && (value = getValueTrue());
 

// if为假
if (!bool) {
 value = getValueFalse();
}

// || 改版

bool || (value = getValueFalse());

```
### 5.对象 或 数组
```js
const getStatusText = (status) => {
  let statusObj = {
     1: '待开通',
     2: '开通准备中',
     3: '开通准备完成'
 };
  return statusObj[status] ? statusObj[status] : '';
}

const getStatus = (status) => {
  let statusArr = ['待开通','开通准备中','开通准备完成']
  return statusArr[status-1] ? statusArr[status-1] : '';
}

```
### 6.includes
```js
if (operate === 'submitAudit' || operate === 'cancelUpdate' || operate === 'uplineApplet') {
  this.handleOperate(operate, row);
  return;
}

//使用includes
const actionArr = ['submitAudit','cancelUpdate','uplineApplet'];
if(actionArr.includes(operate)){
  this.handleOperate(operate, row);
  return;
}

```
### 7.Map
假如：

'待开通' 具有 设置版本setVersion 的操作

'开通准备中' 无操作

'开通准备完成' 具有发布releaseApplet和取消本次开通cancelDredge的操作
```js
const getStatus = (status) => {
  const maps = new Map([
   [1,['待开通',['setVersion']]],  
   [2,['开通准备中',]],  
   [3,['开通准备完成',['releaseApplet','cancelDredge']]],
 ])
  
  const action = maps.get(status);
  let obj = {
    statusText:'',
    statusFn:[],
 }
  obj.statusText = action[0];//  ["待开通"]
  obj.statusFn = action[1] || [];// ['setVersion']
  return obj;
}
getStatus(2) // { statusText: "开通准备中",statusFn: [] }

```

### 其他复杂的Map用法可以参看这边文章
https://segmentfault.com/a/1190000017105791

