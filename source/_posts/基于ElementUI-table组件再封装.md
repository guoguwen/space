---
layout: post
title: 基于ElementUI table组件再封装
subtitle: Vue 组件学习与实践
date: 2018-04-23 17:13:11
author: "Guwen Guo"
header-img: "/img/vue_table.jpeg"
cdn: "header-off"
categories:
        - 学习总结
tags:
        - Vue组件封装
        - JavaScript
        - Egrid
---

## 基于Element-UI Table 组件封装的高阶表格组件

##### 后台系统的开发总是离不开Table组件，现有的组件已经满足不了真实的需求了，Element-UI 只是简单的封装了一下数据展示还是比较方便的，可是遇上复杂的业务逻辑，复杂的DOM操作，原有的Element已经真的饿了。

##### 就有大🐂对原有的Table组件进行封装，其实封装也简单，就是耗费一点时间，任务那么繁重，工作最有效的当然是拿来主义喽。有时间可以慢慢研究一下人家写的代码，说不定还有更优的思路呢？先用吧！

## 引入Egrid (前提条件是后台使用了Element框架)

{% codeblock %}
npm i element-ui -S
npm i egrid -S

import Vue from 'vue'
import Egrid from 'egrid'
Vue.use(Egrid)
{% endcodeblock %}

## 使用Table的地方引入Egrid

{% codeblock %}
<egrid border
    max-height="500"
    column-type="selection"
    :data="data"
    :columns="columns"
    :columns-schema="columnsSchema"
    :columns-props="columnsProps"
    :columns-handler="columnsHandler"
    @selection-change="selectionChange">
</egrid>
{% endcodeblock %}

####  参数详情 
selecttion:     定义表格的类型可选状态
data:           表格渲染的数据源
columns:        表格标题栏渲染的数据源
columnsSchema:  定义columns的某一列，这里的设置会覆盖columnsProps的配置属性
columnsProps:   定义 columns 公共的属性
columnsHandler：可用于在现有的 columns 进行操作，对 columns 进行增删改

## 示例
{% codeblock %}
const columns = [
  {
    label: '日期',
    prop: 'date',
    width: 100
    ...
  },
  {
    label: '姓名',
    prop: 'name',
    minWidth: 100
    ...
  },
  {
    label: '其他',
    component: Btn, // 'el-button'
    listeners: {
      'custom-event' (data) {
        console.log('custom-event', data)
      }
    },
    propsHandler: function ({ row, col, column }) {
      return { row, col, column }
    }
    ......
  }
  ......
]
{% endcodeblock %}

## 结语
其实，有官方文档的，写多了也没用，知道有这个东西就行了，以后用起来方便 [官方地址](https://nlush.com/egrid/#/)