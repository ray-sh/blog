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
The this undefined come from caller
The this [object Object] come from funciton self
20 xxx
16

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
如果只有一个参数，括号也可以省略
[1,2].map(x => x + 1)
```
## 对象
- 不用定义直接生成一个实例对象
```
const age = 'age'
const home='shanghai'
const obj = {
  name: "jack",
  sayhi() {
    console.log('hi')
  },
  display: () => console.log(name),
  [age+ "1"]: 42,//[]里面可以放表达式
  sex: 'male',
  home//等价 home:home,这种写法更简洁
}


console.log(obj)

output：
{name: "jack", age1: 42, sex: "male", sayhi: ƒ, display: ƒ, …}
age1: 42
display: () => console.log(name)
home: "shanghai"
name: "jack"
sayhi: ƒ sayhi()
sex: "male"
__proto__: Object
```
- 属性的destructure
```
const obj = {
  name: "jack",
  sayhi() {
    console.log('hi')
  },
  display: () => console.log(name),
  age: 42,//[]里面可以放表达式
  sex: 'male',
  home: 'home'
}

function dump(obj){
 //这种写法更简单，相比较obj.sex, obj.name
 const {sex,name,home} = obj
 console.log(name,home,sex)
}
dump(obj)

output:
jack home male

//更简单的写法，有点pattern match的概念
function dump2({sex,name,home}){
 console.log(name,home,sex)
}
dump2(obj)

const dumpsex = ({sex}) => sex

//甚至可以包含不存在的属性，如果没有找到会返回undefined，也可以个一个默认值
function dump2({sex,name,home,street='wall'}){
 console.log(name,home,sex,street)
}
//利用该属性可以实现类是key argument
function display({name, page, any ...})
{

}
//更方便的引入package
const {Component, useState} = require('react')
```
- 数组的destructure
```
//array的deconstruct
var [a,b] = [1,2,3]
console.log(a,b)
//skip one
var [a,,b] = [1,2,3]
console.log(a,b)
//no match
const [c,d] = []
console.log(c,d)

//first and rest, very powerful
var [a, ...rest] = [1,2,3,4]
console.log(a,rest)
//copy rest to new array
var newarray = [...rest]
console.log('newarray:', newarray)
//...rest也可以用在object上
const person = {
  name: 'jack',
  age: 11,
  sex: 'mail'
};

var {name, ...rest} = person
console.log(name, rest)
//copy一个新的object,过滤调一些属性
var newPerson = {...rest}
console.log('new person',newPerson)

output:
1 2
1 3
undefined undefined
1 (3) [2, 3, 4]
newarray: (3) [2, 3, 4]
jack {age: 11, sex: "mail"}
new person {age: 11, sex: "mail"}
```
## 尽量使用async/wait来处理异步
```
const fetchData = {} => {
  fetch('http://api.github.com').then(resp => {
    resp.json().then(data => {
      console.log(data)
    });
  });
};

const fetchData2 = async() => {
  const resp = await fetch('github')
  const data = await resp.json()
  
}

```