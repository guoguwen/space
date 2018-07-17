---
title: element-ui之样式修改编译
date: 2018-07-17 17:24:01
header-img: "/img/background-data-microservice.jpg"
cdn: "header-off"
tags:
    - Vue
---

### 改样式改的痛不欲生

###### 最近的项目用的是ElementUI个人感觉后台系统嘛，美观倒不用太特别在意吧，但是产品经理是个偏执狂啊，看着人家的东西都想比别人更好，也不考虑自己的团队实力和精力。硬逼着UI出了一套图，TMD跟框架的色调样式全改了，这样不如重写一套框架自己团队用。可是团队只有两个前端，这不累死人嘛。那就只能改别人的代码喽。

###### 安装ElementUI默认的框架

``` bash
    #npm install element-theme -g
    #npm npm i element-theme-default -D
    #npm i element-theme-chalk
    #et -i    //打开本地下载下来的默认Scss文件修改(当前目录下的element-variables.scss)
    #et       //编译生成
```
###### 寻找theme 文件夹下的 index.css文件就是修改过后的文件了