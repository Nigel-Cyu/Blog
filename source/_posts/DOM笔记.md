---
title: DOM笔记
date: 2018-02-28 16:43:12
tags:
---
### 1.DOM的定义
DOM（Document Object Model）：用来操作html和css的标准编程接口。

### 2.查看元素节点
(1)document.getElementById("Id")：通过 Id 找到对应的元素

(2)getElementsByTagName("标签名")：通过标签获取元素们

(3)getElementByName()：通过name
         注：只有部分标签可以生效。如：表单、表单元素、img、iframe

(4)getElementsByClassName()：通过类名class

(5)querySelector("")：""里面的内容添加正常的各种css选择器
        注: querySelector(“”)只能选择一个标签，即使可以选出一组div，也只返回一组中的第一个
    
    querySelectorAll("")：""里面也是加css选择器，但是他可以选择多个，返回一个类数组
    注：querySelector和querySelectorAll选择的东西是非实时的
<!-- more -->
### 3.节点

获取节点类型 nodeType ：返回数字，根据数字判断类型

    元素节点-----1
    属性节点-----2
    文本节点-----3
    注释节点-----8
    Document------9
    documentFragment-----11

    注：文本节点为文字、空格等，每一个节点都有自己的属性和方法，Document为文档节点，是dom树的根节点
    
    parentNode-->父节点 （最顶端的parentNode为#document） 注：获取的是最邻近的上一级
    childNodes-->子节点们（六种类型节点）
    firstNode-->第一个子节点
    lastNode-->最后一个子节点
    nextSibling-->后一个兄弟节点
    previousSibling-->前一个兄弟节点

    针对元素节点（不包括文本、注释节点）
    parentElement-->返回当前元素的父元素节点(最顶端parentElement是html,document不是元素,IE不兼容)
    Children-->只返回当前元素的元素子节点们，返回类数组
    nodechildElementCount === node.children.length-->当前元素节点的子元素节点个数（IE不兼容）
    firstElementChild-->返回的是第一个元素的节点（IE不兼容）
    lastElementChild-->返回的是最后一个元素的节点（IE不兼容）
    nextElementSibling--> 返回后一个兄弟元素节点（IE不兼容）
    previousElementSibling-->返回前一个兄弟元素节点（IE不兼容）

    节点的四个属性：
    nodeName（节点名称）：1)元素的标签名，以大写形式表示，只读 2)文本#text、注释#comment、文档#document
    nodeValue（节点值）：Text节点或Comment节点的文本内容，可读可写（对于文档节点document和元素节点是不可用的）
    nodeType（节点类型）：该节点的类型，只读
    attributes：Element节点的属性集合

    节点树总结：
    Document和Element是并列关系，所以定义在Document上的东西，Element上不能用
    ByID ：只能document上用
    ByTagName/byClassName/querySelectorAll/querySelector ：Element和document上都能用
    Document.body 指代body；document.head指代head
    Document.documentElement 指代html

### 4.DOM基本操作

(1)增
document.createElement()：创建标签
document.createTextNode()：创建文本节点
document.createComment()：创建注释节点
document.createDocumentFragment()

(2)插
parentNode.appendChild()：将节点插入子节点的尾部，同时具有剪切功能
parentNode.insertBefore(a, b)：读作insert a before b，在b前面插入a

(3)删
parent.removeChild()

(4)替换
parent.replaceChild(new, origin)：new为要替换的新元素，origin为被替换的老元素

### 5.Element节点
(1)属性
innerHTML：将标签拿出来
innerText（火狐不兼容）/textContent（老版本IE不好使）：将文本拿出来

(2)方法
ele.setAttribute()
ele.getAttribute()
ele.className

### 6.DOM其他操作

(1)查看滚动条的滚动距离
    window.pageXOffset/pageYOffset（IE8及IE8以下不兼容）

    document.body/documentElement.scrollLeft/scrollTop（IE8及IE8以下使用）
    用法：兼容性比较混乱，用时取两个值相加，因为不可能存在两个同时有值
        比如 var left = document.body.scrollLeft + document.documentElement.scrollLeft

(2)查看视口尺寸
    window.innerWidth/innerHeight（IE8及IE8以下不兼容）

    document.documentElement.clientWidth/clientHeight（标准模式下，任意浏览器都兼容）

    document.body.clientWidth/clientHeight（适用于怪异模式下的浏览器）

(3)查看元素的几何尺寸
    domEle.getBoundingClientRect() （兼容性很好）
     该方法返回一个对象，对象里面有left,top,right,bottom等属性。left和top代表该元素左上角的X和Y坐标，
     right和bottom代表元素右下角的X和Y坐标。

    domEle.offsetWidth   domEle.offsetHeight

(4)查看元素的位置：与最近的有定位的父级之间的距离
    dom.offsetLeft, dom.offsetTop
      对于无定位父级的元素，返回相对文档的坐标。对于有定位父级的元素，返回相对于最近的有定位的父级的坐标（不在乎自己是否有定位）
    
    dom.offsetParent
      返回自己最近的有定位的父级，如无，返回body， body.offsetParent 返回null

(5)让滚动条滚动：
    window上有三个方法：
        scroll(),scrollTo(),scrollBy();
    三个方法功能类似，用法都是将x,y坐标传入。即实现让滚动轮滚动到当前位置。
    区别：scrollBy()会在之前的数据基础之上做累加。Scroll()和scrollTo是对定点，不累加

### 7.脚本化css（即操作css）

(1)读写元素css属性：（dom中只有style这种方式可以写、更改行间样式css，其余的只能读）
    dom.style.prop

(2)查询计算样式：（只能读，但不要求非得是行间）
    
    window.getComputedStyle(ele,null)    IE8 及 IE8以下不兼容
    ①查看普通元素：第一个参数：要查询的元素；第二个参数：false
    ②查看伪元素：第一个参数:要查看伪元素的元素;第二个参数:要看的什么伪元素（before/after）

    ele.currentStyle     IE独有的属性（只能在IE上用）
    这是某个标签的一个属性（不是window上的）。

    封装兼容性方法
    function getStyle(obj, propStyle) {
        if(obj.currentStyle) {
            return obj.currentStyle;
        }else{
            return window.getComputedStyle(obj, false)[propStyle];
        }
    }
