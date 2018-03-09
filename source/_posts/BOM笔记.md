---
title: BOM笔记
date: 2018-03-02 22:47:16
tags:
---
1.定义
Browser Object Model，定义了操作浏览器的接口

2.BOM对象
    注：由于浏览器厂商不同，Bom对象的兼容性极低，一般情况下只使用其中的部分功能。

① Window   JavaScript 层级中的顶层对象，表示浏览器窗口
  alert()：显示一段消息和和一个确认按钮的警告框。
  clearInterval()：
  clearTimeout()：
  confirm()：返回一个确认框
  moveBy()：相对窗口当前坐标移动指定像素
  moveTo()：将敞口左上角移动到指定坐标
  open()：打开一个新的窗口
  Print()：打印当前窗口的内容
  Prompt()：输入
    属性：
        Closed: 打开的窗口是否关闭
        Opener: 返回对创建此窗口的窗口的引用
        innerHeight: 窗口文档显示区的高度
        innerwidth: 窗口文档显示区的宽度
<!-- more -->
② Navigator  包含客户端浏览器的信息
  方法：
    Javaenable: 规定浏览器是否启用 Java
  属性：
    appCodeName: 只读；返回浏览器的代码名
    appName: 浏览器名字（区别IE和非IE）
    userAgent: 返回电脑或手机详细信息（区别是手机端还是PC端，区别哪个浏览器）

③ Screen  包含客户端显示屏的信息
  属性：
    Avilheight：显示器的高度
    deviceXDPI: 返回显示器，每英寸水平点数
    deviceYDPI: 返回没英寸垂直点数

④ History  包含了浏览器窗口访问过的 URL
  属性：
    History.length: 浏览记录的个数
  方法：
    History.back: 后退一个历史记录
    History.forword: 前进一个历史记录
    History.go(a): 若a=-2，后退两个历史记录，若a=3，前进两个历史记录

⑤ Location   包含了当前 URL 的信息
    属性：
        href： 设置或返回完整的 URL
        hash：做锚点（和a标签类似，但只能一次）
    方法：
        assign()： 加载新的文档
        reload()： 重新加载当前文档
        replace()： 用新的文档替换当前文档