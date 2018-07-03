---
layout: post
title: '''Vue数据无响应解决办法'''
date: 2018-07-03 10:09:22
author: "Guwen Guo"
header-img: "/img/hero.jpg"
cdn: "header-off"
tags: 
    Vue
---

### What Vue 数据改变无响应？

##### Vue在绑定数据时，数组中的值发生改变时界面无响应，最近在写前端界面时遇到的问题，点击div块元素时界面块元素发生变化字体颜色发生改变。

##### 第一反应就是把这些块元素按照顺序存到数组，点击哪个元素传哪个ID，把数组中的这个ID的元素变为true,想法是没有错误的但是写完没有发生改变。之前也遇到过这个情况但是没有仔细研究，当时是一个多规格创建的dom,也是数组的思想，动态添加商品名字，图片，价格库存之类的。也是先把dom写出来用class的办法解决，用到时把display属性变为block。可是传的ID把数组中的ID变为true。界面仍然没有响应

##### 没有响应的代码
``` bash
    addSku(args){
        this.arr[args] = true;
        ...
    }
```
##### 有问题就看官方文档，在深入响应式原理可以看到，Vue 不允许在已经创建的实例上动态添加新的根级响应式属性 (root-level reactive property)。然而它可以使用 Vue.set(object, key, value) 方法将响应属性添加到嵌套的对象上：Vue.set(vm.someObject, 'b', 2)有时你想向已有对象上添加一些属性，例如使用 Object.assign() 或 _.extend() 方法来添加属性。但是，添加到对象上的新属性不会触发更新。在这种情况下可以创建一个新的对象，让它包含原对象的属性和新的属性：
``` bash
// 代替 `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```
##### 也有一些数组相关的问题，之前已经在列表渲染中讲过。那上面的代码就变这样了

``` bash
    addSku(args){
        this.$set(this.arr,args,true);
    }
``` 

##### 就这样解决了问题，看来还是需要深入学习知识，肤浅的会用不会长远