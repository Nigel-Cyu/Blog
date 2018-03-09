---
title: Javascript-事件
date: 2018-03-01 21:57:01
tags:
---
### 1.定义
> 事件：用户和电脑之间的交互。如：用户点击鼠标、敲键盘等动作，我们需要给这些动作一个回复，即绑定一个回调函数。
    注：回调函数：满足一定条件后执行的函数

### 2.如何绑定事件

(1)ele.onxxx = function (event) {}    注：this指向ele元素本身
   ①兼容性很好，但是一个元素只能绑定一个处理程序（当一个事件想绑定多个处理函数时就不行了）
   
   ②基本等同于写在HTML行间上
         
   ③同setInterval()一样，其他部分监听，同步执行
<!-- more -->
(2)obj.addEventListener(type, fn, false);    注：this指向ele元素本身
   ①参数type：事件类型；fn：处理函数；false：冒泡，true：捕获
   ②IE8及IE8以下不兼容
   ③可以为一个事件绑定多个处理程序
   ④唯一可以执行事件捕获的
      注：函数fn只能是函数声明function text(){}，不能是函数表达式var text = function(){}

(3)obj.attachEvent('on' + type, fn);    注：this指向window
   IE独有，一个事件同样可以绑定多个处理程序
    
   特点：同一个函数可以绑定多次，绑定多少次，就调用多少次

(4)封装兼容性的 addEvent(elem, type, handle) 方法
    function addEvent(elem, type, handle) {
        if(elem.addEventListener){
            elem.addEventListener(type, handle, false);
        }else if(elem.attachEvent){
            elem['temp' + type + handle] = handle;    //给回调函数起一个名字为elem[type + handle]，这样方便解除事件绑定的函数
            elem[type + handle] = function () {
                elem['temp' + type + handle].call(elem);   //然后在将回调handle的名字改为比较专业的名字
            }
            elem.attachEvent('on' + type, elem[type + handle]);
        }else{
            elem['on' + type] = handle;
        }
    }

### 3.解除事件处理程序

(1)ele.onclick = false/''/null;

(2)ele.removeEventListener(type, fn, false);
   注：fn一定是起始绑定的事件处理函数的引用

(3)ele.detachEvent(‘on’ + type, fn);
   注:若绑定匿名函数，则无法解除

### 4.事件冒泡、捕获

(1)事件冒泡：
  ①结构上（非视觉上）嵌套关系的元素，会存在事件冒泡的功能，即同一事件，自子元素冒泡向父元素。（自底向上）
   即：在父级定义某一事件（如：click），点击子元素，会触发父元素定义的事件，如同这一事件从子元素冒泡到父元素一样
  
  ②任何浏览器都有

(2)事件捕获：
  ①结构上（非视觉上）嵌套关系的元素，会存在事件捕获的功能，即同一事件，自父元素捕获至子元素（事件源元素、顺序与事件冒泡正好相反）
   即：同事件冒泡一样，只不过顺序是从父元素到子元素，而冒泡顺序是从子元素到父元素。
  
  ②IE没有捕获事件，只有谷歌有（webkit内核的有）

(3)触发顺序:先捕获，后冒泡
    注： focus，blur，change，submit，reset，select 等事件不冒泡

(4)冒泡和捕获只能二选一，不能同时存在

### 5.取消冒泡和阻止默认事件

(1)取消冒泡
    W3C标准 event.stopPropagation(); 但不支持IE8及IE8以下以下版本

    event.cancelBubble = true; IE独有

    封装取消冒泡的函数 stopBubble(event)
    function stopBubble(event) {
        if(event.stopPropagation){
            event.stopPropagation
        }else{
            event.cancelBubble = true;
        }
    }

(2)阻止默认事件
  默认事件 — 表单提交submit，a标签跳转，右键菜单等
  
  ①return false; 以对象属性的方式注册的事件才生效，即div.onclick = function(){}形式；但是这种形式也可以用event.preventDefault()

  ②event.preventDefault(); W3C标注，IE8及IE8以下不兼容

  ③event.returnValue = false; 兼容IE，但在谷歌上也好使

    封装阻止默认事件的函数 cancelHandler(event)
    function cancelHandler(event) {
        if(event.preventDefault){
            event.preventDefault();
        }else{
            event.returnValue = false;
        }
    }

### 6.事件对象

(1)定义：系统在调用事件处理函数的时候，会传进来一个事件对象event，该对象上存了许多关于事件的信息和方法，
        每执行一次事件，会产生一个新的事件对象记载当前事件的一些新信息

(2)鼠标事件的事件对象部分内容
  ①cancelBubble: true; 取消冒泡 

  ②clientX和clientY是当前鼠标相对于浏览器边框的坐标（能看见的浏览器的视口的坐标，与滚动条无关）
  
  ③offsetX和offsetY设置的是鼠标相对于目标事件的父元素的内边界的X和Y坐标（如：div.onclick = function(){} ，目标事件click，父元素div，也就是说鼠标相对于div的内边界的坐标。即使在事件冒泡中，点击li，offset也是相对于自身li的定位）
  
  ④screenX和screenY与client类似，只不过screen相对的是带有滑轮的整个真正的浏览器的边框
  
  ⑤target事件源对象
  
  ⑥button  0\1\2  鼠标左键\滑轮\右键

(3)event || window.event 用于IE
   因为IE的事件对象传到window上，谷歌通过事件处理函数将事件对象传到形参e上。
   
   获取事件对象的兼容性写法：
        div.addEventListener('click', function(e){
            var event = e || window.event;
        }, false)

(4)事件源对象
    在浏览器用鼠标真实点到的对象，事件对象里target属性表示事件源对象是谁
        注：event.target   火狐独有的
            event.srcElement  Ie独有的
            这俩chrome都有
        兼容性写法：
            var target = event.target || event.srcElement;

### 7.事件委托机制
    利用事件冒泡，和事件源对象进行处理
        优点：
            ①性能：不需要循环所有的元素一个个绑定事件
            ②灵活：当有新的子元素时不需要重新绑定事件

### 8.事件分配

(1)鼠标事件
用button来区分鼠标的按键，0/1/2
      注：写事件的时候不用写头峰式写法，一律小写
①click：点击事件
   click = mousedown + mouseup（即触发一个mousedown事件，又触发一个mouseup事件，系统会自动生成一个click事件，如果鼠标按下不抬起就不会触发click事件）
   click不能监听鼠标右键，可以用mousedown

②mousedown：鼠标按下，鼠标左右键都可以监听

③mouseup：鼠标抬起

④mousemove：鼠标移动

⑤mouseover：鼠标悬停（如：鼠标移入某个div时）

⑥mouseout：鼠标移出（如：鼠标移出某个div）通过mouseover和mouseout可以实现伪元素hover的效果

⑦contextmenu：右键出现菜单的事件（是默认事件）

(2)键盘事件
keydown keyup keypress
     注：keydown + keyup != keypress
     注：键盘事件按下触发多次，鼠标触发多次
  
  触发顺序：keydown > keypress > keyup
  
  keydown和keypress的区别：
      keydown 可以响应任意键盘按键，keypress只可以相应字符类键盘按键
      keypress返回ASCII码，可以转换成相应字符，利用fromcharcode()

键盘事件对象：KeyboardEvent
    charcode（keypress中有）：代表字符键的ASCII码。

(3)文本操作事件
①input：可以获取value的值
       
②focus、blur：文本框鼠标移入移出效果

③change：对比focus和blur前后文本框里面内容是否变化

(4)窗体操作类(window上的事件)
①Load：
   window上的load事件:当整个文档加载并执行完毕,执行window.onload = function(){}
          所以可以经script标签写在任何地方，但是效率会严重降低。通常将它写在
          最后，效率是最高的
       Document.write()的危害：document.write用来打印html代码（好处）；
           当整个文档加载完之后再执行document.write()会将之前的文档流全部清
           除掉（即所有的html代码）（危害）

②scroll（只能在window上使用）：当滚动条滚动可以触发该事件
