---
layout: post
title: React里面的javasciprt
date: 2019-12-25上午 10时03分23秒 +0800
categories:
---

## Javascipt的变量
- 变量的作用域
  - var定义的变量会在编译的时候提升到最开始
  - 函数里面的变量在编译的时候确定其引用，在执行的时候确定值
```
//Block scope
{{{{var a = 1}}}}
console.log(a)
console.log(b)
if (true)
{
  //Block scope
  var b = 2;
}
for(var i =0; i< 4; i++)
{
  //block scope
}
console.log(b)
console.log(i)
function add(c,d){
  //function scope,这里的b的值不会外漏
  var b = 111;
  console.log(`add function, value b is ${b}`)
}
add(1,2)
console.log(b)

输出
1
VM68:18 undefined
VM68:24 2
VM68:25 4
VM68:28 add function, value b is 111
VM68:31 2
```

  - var是有副作用的，除非有特殊目的，一般用let， 表示不变的常量用const
  - const只能保证变量的ref不被修改
```
const a = 1
a=2//这里会报错，a已经指向1，不能修改它指向2

const b = [1,2]
b.push(3)
//const只能保证b的引用本身不被修改，引用所指向的内容还是可以修改

```
## 函数
- 不同的函数定义方法会导致this的不同

```
function add (a,b){
  console.log(`The this ${this} come from caller`)
  return a + b
}
add(1,2)

let sub = function(a,b){
  console.log(`The this ${this} come from caller`)
  return a-b
}
sub(3,1)

let mul = (a,b) => {
  //这对于定义用于处理事件的回调函数非常有用
  console.log(`The this ${this} come from funciton self`)
  console.log('xxx')
  return a * b
}

Output
The this undefined come from caller
VM96:14 The this undefined come from caller
VM96:19 The this [object Object] come from funciton self
VM96:20 xxx
VM96:23 16

const test = {
  f1: function(){
    //this come from caller
    console.log('f1',this)
  },

  f2: () => console.log('f2',this)
}

test.f1()
test.f2()
f1 {f1: ƒ, f2: ƒ}
f2 {id: "PLAYGROUND"}
```

- 箭头函数可以简写
```
let add = (a,b) =>{
    return a + b
}
可以简写为
let add = (a,b) => a + b
```