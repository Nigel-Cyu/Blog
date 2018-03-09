---
title: JavaScript基础笔记
date: 2018-02-24 15:45:13
tags:
---

### 1.JavaScript的特点
(1)用于向HTML页面添加交互行为
(2)脚本语言,用于编写客户端脚本
(3)单线程
(4)解释性语言，解释一行执行一行
(5)可以跨平台

### 2.typeof 返回值类型
number(数字，NaN), string(字符串), boolean(true，False), undefined(undefined), object(null，数组，对象), function(function)
    注：(1) typeof 打印出来的变量类型都是字符串的形式。如："number" "undefined"
        (2) typeof(null)-->"object" 意义为null是“非对象”; 而typeof(undefined)-->"undefined"

<!-- more -->
### 3.函数
(1)系统函数
    ParseInt() 转换为整数
    isNaN() 判断是否为数字
    typeof() 判断数据类型
    eval() 计算表示式值

(2)自定义函数
    Function 函数名() {javaScript代码}
    Function 函数名(参数1，参数2) {javaScript代码  return 返回值(可选)}

### 4.引用值和原始值的区别
(1)复制时复制的是什么？
    原始值：复制的是变量真正的值
    引用值：复制的是变量的地址
    
    例如：var num=123; num1=num; num=124; document.write(num1);
          打印出num1的值为123，复制的是原始的值123，所以num的值改变不会影响到num1
    
    例如：var arr=[1,2,3,4]; var arr1=arr; arr.push(5); documemt.write(arr1);
          打印出arr1的值为1,2,3,4,5，复制的是引用值arr的地址，所以arr改变，arr1也改变
          
          var arr=[1,2,3,4]; var arr1=arr; arr=[1,2,3]; documemt.write(arr1);
          打印出arr1的值为1,2,3,4。此时，arr放弃原来房间，又开辟出一个房间存1,2,3
          通过这两个可以看出来，arr.push()是在原数组上添加成员，而arr=[1,2,3,4];后arr=[1, 2, 3];是重新赋值了一个数组

(2)是否可截断？（通过减小length的值，是否可以减少内容或长度）
    原始值：不可改变。（在栈中存储，栈中的值不可能改变，要改变，只能改变地址，另辟房间）
    引用值：可改变。（堆中存储）
    
    例如：var arr=[1,2,3,4]; arr.length=2; document.write(arr);
          打印出arr的值为1,2，数组的长度可截断。
       
    例如：var str=”abc”; str.length=2; document.write(str);
          打印出str的值为abc，字符串的长度不可截断。

(3)原始值的比较是数值的比较；引用值的比较是引用的比较，当且仅当他们引用同一个对象时才相等

### 5.类型转换
(1)显示类型转换
    
    Ø  Number(n)：将n转换为数字类型   注：toFixed(n) 是保留 n 位小数
       true-->1; false-->0; undefined-->NaN; ""-->0; "abc"-->NaN; "123abc"-->NaN; []-->0; {}-->NaN
    
    Ø  parseInt(string,radix)：转换为整型
       "123abc123"-->123（遇到字母，就停止了，不再看后面的东西，直接返回前面的123）
       radix是基底：表示前面的数是几进制的数，取值为2-36之间的数字
    
    Ø  parseFloat(string)：转换为浮点型
        123.1.2-->123.1（只能找到一个小数点）; "123abc123"-->123（遇到字母后面的就不看了）
    
    Ø  toString()：转换为字符串类型（对象上的一个方法，任何数据类型都可以使用）
        使用方法有点改变：不是用括号，而是用.toString函数
        
        注意：①undefined和null不能调用.toString，否则系统会出错
              ②radix：以十进制为基础转换为目标进制（也是字符串类型）

    Ø  String(max)：任何类型都转化为字符串类型（类似number）
    Ø  Boolean()：将任何类型转化为boolean类型（类似number）
    
(2)隐式类型转换
    
    Ø  isNaN()：判断是不是非数。（先转换为number类型，再判断是不是非数NaN）
    
    Ø  ++/--  +/- (一元正负)：自动转化为number类型，再进行++/--/+/- 的运算

    Ø  + ：任何东西+字符串=字符串

    Ø  * / %（乘除）：先转化为number类型，再进行运算

    Ø  &&||！ :首先会将左右两边的式子转换为boolean来判断是否为真，最后返回的是原来的类型，不是boolean
       
    Ø  < > <= >= == != ：会将符号两边转化为相同的类型进行比较

### 6.作用域
(1)作用域定义：变量（变量作用域，又称上下文）和函数生效（能被访问）的区域。
   注：Javascript只有函数可以产生作用域。

(2)[[scope]]：每个Javascript函数都是一个对象，对象中有些属性我们可以访问，有些不可以，这些属性仅供Javascript
              引擎存取，[[scope]]就是其中一种。（即：每个函数都有一个作用域[[scope]]）
              [[scope]]就是我们所说的作用域，其中存储了运行期的上下文

(3)作用域链：[[scope]]中存储的执行期上下文对象的集合，这个集合成链式连接，我们把这种链式连接叫做作用域链。

(4)执行期上下文：当函数执行时，会创建一个称为执行期上下文的内部对象。一个执行期上下文定义了一个函数执行时的环境
              （调用函数时，所执行的东西，如：定义变量，调用其他函数等即为执行期上下文。但是，如果只是定义了函数，
               则不产生执行期上下文）
        注意：函数每次执行时对应的执行期上下文都是独一无二的，所以多次调用一个函数会导致创建多个执行期上下文，
              当函数执行完毕，执行期上下文销毁。

(5)查找变量：从作用域链的顶端依次查找
  
  例. function a () {
        function b () {
            var b = 234;
        }
        var a = 123;
        b();
        docunmet.write(a);
      }
      
      var glob = 100;
      b();
    
    定义函数a时，则会将a能看见的全局变量和函数放在数组scope[0]处，当调用函数a时，会产生执行期上下文放在 scope 中，
    全局变量下移到scope[1]中，执行期上下文放在scope[0]中。当查找变量a时，先从scope[0]中找，找不到再从scope[1]找。

    函数a里面定义了函数b，函数b能看见的全局变量和函数=a的劳动成果+a的全局上下文（即为a的执行期上下文），当函数b执行
    时，会产生执行期上下文放在scope[0]位置，之前的依次往下窜。

    当函数b执行完，b的执行期上下文销毁，当函数a执行完，a的执行期上下文被销毁。

### 7.闭包
(1)闭包定义：当内部函数被保存到外部时，将会生成闭包。

(2)闭包坏处：导致原有作用域链不释放，造成内存泄露。
   内存泄露：不断向内存里装东西，导致内存满了，不能再装东西，相当于内存泄露一样。而闭包会导致作用域链不释放，内存泄露。
   
   例. function a () {
         function b () {
            var b = 234;
            console.log(a);
         }
         var a = 123;
         return b;
       }

       var glob = 100;
       var c = a();
       c();
       函数a执行的结果是返回函数b，而b被赋给c，然后调用函数c，也就是函数b。此时函数a的执行期上下文已经被销毁，但仍然能
       打印出 a = 123。原因是函数a执行和函数b定义的空间相同，当a执行完，其执行期上下文释放后，b的也会被释放，但是我们
       将函数b赋予给了c，b和c具有一样的指向，所以c能访问到变量a。

(3)闭包作用
    Ø  实现公有变量。例如：函数累加器（闭包作用域里的变量都是共有变量，每次访问的都是同一个）
    Ø  可以做缓存
    Ø  可以实现封装，属性私有化

### 8.立即执行函数

(1)定义：此类函数没有声明，在执行一次后就被释放，适合做初始化工作。
    产生原因：一些函数只使用一次，定义函数太浪费，不定义函数则不够模块化。

(2)结构：①(function () {})()   ②(function () {} ())
        注：两种写法功能一样，但是第二种效率更高
    
    立即执行函数里面定义的变量，在函数执行完就会被销毁，不能在访问；但是在函数外面定义的函数，在立即函数里面改变的话，可以访问

(3)传参数：(function (形参) {函数体} (实参))

(4)用途：立即执行函数解决闭包问题
    例. function a () {
           var arr = [];
           for(var i = 0;i < 10;i++){
               arr[i] = function () {
                   console.log(i);
               }
           }
           return arr;
        }

        var b = a();
        for(var i = 0;i < b.length;i++){
            b[i]();
        }
    结果打印出 10 10 10 10....
    函数a里 arr[i] 可以访问外面传进来的参数i，通过函数b的接收产生闭包，每次 b[i]() 执行都会访问真实的 i 值，即最终最新的 i 值。

    使用立即执行函数
        function a () {
            var arr = [];
            for(var i = 0;i < 10;i++){
                (function (index) {
                    arr[index] = function () {
                        console.log(index);
                    }
                })(i);
            }
        }
    使用立即执行函数后，将共有的全局变量i当做实参传给index，此时index相当于普通变量，相当于 i 值被锁定在 index 里。

### 9.预编译
    ①创建AO对象。
    ②找形参和变量声明，将变量和形参作为AO属性名，值为undefined。
    ③将实参值和形参相统一。
    ④在函数体里找函数声明，将函数声明的名字作为AO属性名，值赋予函数体。

    例. var a = 123;
        function test () {
            document.write(a);
            a = 123;
            if(!!undefined) {
                var a = 0;
            }
            document.write(a);
        }

        test();
    预编译：AO = {a:undefined}
    解释执行：AO = {a:-->123}
    打印：undefined 123
    注：查找顺序从作用域顶端，自己的执行期上下文有，就不看别的。而在该函数里第一次打印a之前，预编译过程a已经有值为
        undefined，所以打印undefined而不是234
    注：if里的条件与预编译无关，只有执行时才有关系 。即预编译过程看不出来这种逻辑关系

### 10.创建对象的方法
Ø  对象字面量 ： var obj = {};

Ø  构造函数
    ①系统自带 new Object()/Array()/Number()/Boolean()/Date();
    
    ②自定义
     例.  function Person (name, age, height) {
             //var this = {};
             this.name = name;
             this.age = age;
             this.height = height;
             this.say = function () {
                 console.log(this.name);
             }
             this.eat = function () {
                 this.height++;
             }
          }
          var cnz = new Person("chu", 20, 174);
    
    自定义构造函数(1)函数最前隐式加上var this = {}; (2)执行this.xxx = xxx; (3)隐式返回this

    注：Ø  当没有构造函数时，预编译时执行期上下文中的this隐式指向window，当没有new时，function只是普通的函数；
        Ø  当有new时，在person函数第一行隐式产生var this = {};此时，预编译时，this的指向由window变为{}，后面
           的语句相当于给this加属性，然后，在最后一步隐式的返回this
        Ø  如果Person返回的是引用值(比如 that )时，则不返回this；如果Person返回的是原始值(比如变量 a )时，默认
           返回this（即：new操作符一定要返回对象）

Ø  var obj = Object.create (原型)

### 11.原型链

    function Person() {}
    Person.prototype.name = 'nigel';
    Person.prototype.age  = 20;
    Person.prototype.job  = 'web Engineer';
    Person.prototype.sayName = function() {
        alert(this.name);
    }
    var person1 = new Person();
    person1.sayName(); // 'nigel'

(1)原型对象    
    Ø  每个对象都有 __proto__ 属性，但只有函数对象才有 prototype 属性。

    原型对象，顾名思义，它就是一个普通对象。
    在默认情况下，所有的原型对象都会自动获得一个 constructor（构造函数）默认属性，这个属性(是一个指针)指向 prototype 
    属性所在的函数（Person）。

        person1.constructor == Person
        Person.prototype.constructor == Person
    
    Ø  在 Person 创建的时候，创建了一个它的实例对象并赋值给它的 prototype，原型对象（Person.prototype）其实是构造
       函数（Person）的一个实例。

    Ø  凡是通过 new Function() 产生的对象都是函数对象，所以Function.prototype 是函数对象，且没有 prototype 属性。

(2)__proto__
    JS 在创建对象(不论是普通对象还是函数对象)的时候，都有一个叫做__proto__ 的内置属性，用于指向创建它的构造函数的原型
    对象，即 person1.__proto__ == Person.prototype 。

    Number、String、Boolean、Array、Function、Object的__proto__都是 Function.prototype，因为它们都是函数对象，由
    Function 所构造出来的。
    Function.prototype是一个空的函数，且Function.prototype.__proto__ === Object.prototype。

### 12.call/apply
    作用：改变this指向。
    区别：后面传的形参形式不同。

    function test (a, b) {
        console.log(this.name);
        console.log(a + b);
    }

    window.name = 'abc';

    var obj = {name: 'aaa'};
    test.call(obj, 1, 2);
    //test.apply(obj, [1, 2]);
    
    执行text函数时传入obj，会将text()函数里面的this更改为obj，所以会打印obj的name。
        注：text()和text.call()作用一样，都是执行该函数。
    apply与call的第一个参数用法一样，就是后面的参数形式不一样，apply传的是数组。

### 13.继承

(1)传统形式
    方法：一层一层的原型链
    缺点：过多的继承了没用的属性，思维价值高、实用价值低。

(2)借用构造函数
    function Foo (name, age) {
        this.name = name;
        this.age = age;
    }
    function Son (name, age) {
        Foo.call(this, name, age);
    }
    var son = new Son('nigel', 20);

    缺点：①没有实现真正意义上的继承，不能继承借用构造函数的原型；②每次构造一个对象都需要调用两个构造方法，多走一个函数。

(3)共享原型
    function inherit(p, c) {
        c.prototype = p.prototype;
    }
    P.prototype.name = "nigel";
    function P () {}
    function C () {}
    inherit(P, C);
    console.log(new C().name);

    缺点：C 和P共用一个原型，一个的原型改变，则另一个原型也会改变。

(4)圣杯模式
    function inherit (P, C) {
        function F () {}
        F.prototype = P.prototype;
        C.prototype = new F();
        C.prototype.constructor = C;
        C.prototype.uber = P.prototype;   //此行可以省略，这只是为了找到C.prototype最终的源头
    }

    Foo.prototype.age = 20;
    function Foo () {}
    function Son () {}
    inherit(Foo, Son);
    console.log(new Son().age);   //输出20

    Son.prototype.name = "nigel";
    console.log(Foo.prototype.name);   //输出undefined，C.prototype是F类的对象，该对象的原型和P.prototype相等。
                                         所以Son.prototype改变，Foo.prototype不会改变。
    
    高级写法：var inherit = (function () {
                var F = function () {};
                return function (P, C) {
                         F.prototype = P.prototype;
                         C.prototype = new F();
                         C.prototype.constructor = C;
                         C.prototype.uber = P.prototype;
                       }
             }())
        函数所需要的私有化的东西，通常放在闭包里，如 F。

### 14.命名空间
(1)作用：管理变量，防止污染全局，适用于模块化开发。
(2)产生原因：当不同的人编写js代码时，总是会有定义重复变量的时候，这就会影响变量的使用。
    
    var store = {
        eat: {},
        wear: {
            shoes: {
                name: "adidas"
            },
            pants: {
                name: "supreme"
            }
        },
        live: {}
    }
    一个软件由不同部门负责，每一个部门又由不同的人负责，这样结构更规范好看。
    当调用某个变量名字过长，可以新定义一个变量来接收。

### 15.对象的枚举
(1)for…in循环
格式：for(var prop in obj){
      }
      其中prop是变量，obj是对象。prop每一次循环会充当对象里的一个属性。  注：prop变量类型是String

(2)hasOwnProperty 过滤原型
对象.hasOwnProperty(属性)，如果属性是原型里面的，则返回false，如果属性是构造函数里面的，则返回true。所以把该语句放在if语句中，即可过滤原型。

(3)in操作符
格式：属性 in 对象
   如果属性（包括原型链上的属性）在对象里面返回true，不在，则返回false。
   即：只要是对象能访问到的属性都返回true。
   注：属性是字符串类型，用时需要加单引号

(4)instanceof
格式：对象 instanceof 构造器
    表示前面的对象是不是后面的构造器构造出来的，是则返回true，不是则返回false
    注：构造器包括对象的构造函数和所有的原型对应的构造函数（如原型object.prototype的构造函数Object）
    注：instanceof与constructor的区别：
        instanceof的构造器是原型链上所有原型的构造函数，而constructor只是最近的构造函数

### 16.属性的表示方法
(1)对象.属性名
如：obj.name  son.xing

(2)对象[‘属性名’]
如：obj[‘name’]  son[‘xing’]
  注: 二者的功能相同，但是只要用了方法(1)，系统最终会转化为方法(2)
  注：因为对象的属性名都是字符串类型，所以所以方法(2)会有一对单引号

### 17.arguments.callee 和 函数名.caller
(1)arguments.callee：表示本函数的引用，即函数的名字
    用处：在立即执行函数里面无法找到函数名字时使用。
    例如：求5的阶乘的立即执行函数
        
        var num = (function (n) {
            if(n === 1) {
                return 1;
            }
            return n * argument.callee(n-1);   //想要利用递归，再次调用这个函数
        }(5))

(2)函数名.caller：表示当前这个函数是在哪个函数里面调用的

### 18.克隆
(1)浅层克隆：针对原始值
    
    function clone (parent, child) {
        var child = child || {};
        for(var prop in parent){
            if(parent.hasOwnProperty(prop)){
                child[prop] = parent[prop];
            }
        }
    }

(2)深度克隆：针对引用值

   function deepClone(origin, product) {
        var strFunc = Object.prototype.toString;
        var arrStr = '[Object Array]';
        for(var prop in origin) {
            if(origin.hasOwnProperty(prop)) {
                //处理clone引用值问题
                if(typeof(origin[prop]) != null && typeof(origin[prop]) == 'Object'){
                    if(strFunc.call(origin[prop]) === arrStr){
                        product[prop] = [];
                    }else{
                        product[prop] = {};
                    }
                    deepClone(origin[prop], product[prop]);  //递归到最后克隆的不是对象或数组，而是原始值
                }else{
                    product[prop] = origin[prop];
                }
            }
        }
    }

### 19.数组
(1)数组的创建
    1) 数组字面量：如：arr = [1, 2, 3];
    2) arr = new Array(123, 2);
>注： 当arr = new Array(n)的形参只有一个时，表示申请一个长度为n的数组[undefined*n]，并且n必须是整数，不能是小数。

(2)数组常用的方法

    #改变原数组
        reverse：数组反转。如：arr = [1, 2, 3];变为arr = [3, 2, 1]
        
        sort：排序
            ·默认：升序排列
            ·定义自己想要的排序
                回调函数：当满足一定条件时，系统自己调用的函数。
                形参a、b表示数组里面任意的两个数，当返回的数是负数时，让前面的数在前；当返回的数是正数时，让后面的数在前。
            升序 arr.sort(function (a, b) {
                    if(a > b) {
                        return 1;             简化为=>   arr.sort(function (a, b) {return a - b;});
                    }else{
                        return -1;
                    }
                }); 
            降序 arr.sort(function (a, b) {
                    if(a > b) {
                        return -1;            简化为=>   arr.sort(function (a, b) {return b - a;});
                    }else{
                        return 1;
                    }
                }); 
        
        push：在数组的最后一位添加任意个数据
        pop：从数组末尾删一位而且只能删一位，而且这里的删是剪切到arr.pop()中，可以保留到num = arr.pop()
        shift：从数组的头开始删，每次删一位
        unshift：从数组的头加数，可以一次加任意个
        
        splice：arr.splice(pos, len, a, b, c) 截取
            其中pos是从第几位截取，len是截取几位，a, b, c是截取后要插入的数字(任意个)，同时arr.splice返回的是所截取的东西

    #不可改变原数组
        concat：把a数组和b数组相连接,返回一个新数组，且a、b不变
        join：用join()里面的参数将数组每一位连接起来
        split：与join正好相反，是根据split()里的内容将一大串字符串拆成很多个小的字符串

### 20.类数组
    利用对象的属性名模拟数组的特性
>  例. 问obj[1]到obj[6]都是什么？
        var obj = {
            "2": "b",
            "3": "c",
            "4": "d",
            "length": 3,
            "push": Array.prototype.push
        }
        obj.push("e");
        obj.push("f");
       
       首先要清楚：Array.prototype.push = function (num) {
                    this[this.length] = num;
                    this.length++;
                  }
        其次，当添加 e、f 时，实际上是obj[3] = "e"和obj["4"] = "f",同时length++。
        所以obj[0]到obj[6]分别是undefined undefined b e f undefined undefined ,最终length = 5。

### 21.es5严格模式
两种用法：
  ①全局的：在 <script></script> 第一行写 "use strict"; 这样所有的js代码都严格遵循es5的严格模式。
  ②局部的：只想在某个函数里用时，只需要在这个函数里第一行加 "use strict"; 即可

不支持：
  with, arguments.callee, func.caller, 变量赋值前必须声明、局部this必须被声明（person.call(null/undefined)赋值什么就是什么）、拒绝重复属性和参数

### 22.日期对象

(1)Date：是系统内部提供的构造函数（和Array相似），内部有各种属性，可通过new创建对象。

(2)内部函数
Date() getDate() getDay() getFullYear() getMonth() getHours() getMinutes() getSeconds()
setDate() setMonth() setFullYear() setHours() setMinutes() setSeconds() setTime()

    注：getTime不具有实时性，创建一个Date对象时只能记录当前时刻，无论调用多少次getTime，都返回创建该对象时的时间，只有再创建另一个     对象时，才能返回另一个时间。

