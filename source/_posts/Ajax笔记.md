---
title: Ajax笔记
date: 2018-03-05 22:24:02
tags:
---
### 1.同源策略：
同域名、同协议、同端口

### 2.封装Ajax函数

    function ajax(method, url, flag, data, callBack) {
        var xml;
        if(window.XMLHttpRequest()) {
            xml = new window.XMLHttpRequest();
        }else{
            xml = new window.ActiveXObject("Microsoft.XMLHttp");
        }
        method = method.toUpperCase();  //全部转换大写
<!-- more -->
        if(method === 'GET') {
            xml.open(method, url + '?' + data, flag);  //建立对服务器的调用
            xml.send();                                //向服务器发送给请求
        }else if(method === 'POST') {
            xml.open(method, url, flag);
            xml.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
            xml.send(data);
        }
        xml.onreadystatechange = function () {
            if(xml.readyState === 4) {
                if(xml.status === 200) {
                    callBack(xml.responseText);
                }else{
                    alert('error');
                }
            }
        }
    }

### 3.URL

(1) URL：统一资源定位符
    URI：统一资源标识符   注：URL、URN是URI的子集
    URN：统一资源名称   

(2) url的构成 ——> 协议://域名（端口号、参数、查询等）

    协议:http / https   注：https中的s就是ssl安全协议，被加在tcp和各应用层之间

    域名: www.zhidao.baidu.com

    端口: 80/ 90 / 3000  等    注：一般默认 80 或者 8080

(3)浏览器输入一个URL地址后发生的事情

1)域名解析：浏览器获取url地址，向操作系统获取url对应的IP地址，操作系统查询DNS（先查询本地HOST文件，没有则查询网络）获得对应的IP地址
    注：在这个过程中，如果浏览器询问DNS时无法找到对应的IP地址，就会向它的上级服务器询问，这样一层一层向上级找，最高达到根节点，直到找到或找不到为止。

2)确认好IP地址和端口号后，则可以向该IP地址对应的服务器的该端口号发送TCP连接请求

3)服务器收到TCP连接请求后，回复可以连接请求

4)浏览器收到回传的数据后，还会向服务器发送数据包，表示三次握手结束

5)三次握手成功后，开始通讯，根据HTTP协议的要求，组织一个请求的数据包，里面包含请求的资源路径、用户身份等信息。发送后，服务器相应请求，将数据返回给浏览器，数据可以是html网页，也可以是图片或脚本程序。如果资源路径指定的资源不存在，服务器会返回404错误；如果返回的是一个页面，则根据页面里的外链URL地址，重复上述步骤，再次获取。

6)渲染页面，并开始响应用户的操作

7)窗口关闭时，浏览器终止与服务器的连接(四次挥手)


(4)TCP

TCP表示传输控制协议，是一种面向连接的、可靠的、基于字节流的传输层通信协议。

TCP三次握手：所谓三次握手，就是建立一个TCP连接时，需要客户端和服务器之间发送三个数据包。
    
    第一次握手：建立连接时，客户端发送syn包(syn=j)到服务器
    第二次握手：服务器收到syn包，向客户端发送ACK包(ack=j+1),并且发送一个自己的syn包(syn=k)，若不同意发送RST+ACK包
    第三次握手：客户端收到服务器ACK+SYN包，向服务器发送确认ACK包(ack=k+1)

    需要三次握手的原因：防止客户端的连接请求因错误延时而导致服务器资源浪费，确保可靠性。

TCP四次挥手：TCP连接的拆除需要发送四个包，客户端或者服务器端均可主动发起挥手动作。

    第一次挥手：客户端发送一个Fin报文，告诉服务器关闭连接
    第二次挥手：服务器收到这个包后，发送ACK包给客户端，告诉他未准备好，此时客户端进入FIN_WAIT状态，等待服务器的Fin报文
    第三次挥手: 服务器向客户端发送Fin报文，服务器断开与客户端的连接
    第四次挥手：客户端收到Fin报文后，发送一个ACK包并进入FIN_WAIT状态，等待了2MSL后仍未收到回复就表示挥手结束

    注：SYN（建立连接）、ACK（ACK=1时，确认字号才有效）、RST（释放/拒绝连接）、FIN（释放一个连接）

TCP(传输控制协议) 和 UDP(用户数据协议)的区别
    a.tcp基于连接，udp基于非连接
    b.tcp保证可靠交付，udp尽最大努力交付
    c.tcp首部20字节，udp首部8字节
    d.tcp面向字节流的，udp面向数据报的


### 4.HTTP、HTTPS

(1)HTTP 超文本传输协议，运用极其广泛；HTTPS是HTTP的安全版
    注：HTTP 协议是TCP/IP协议族中的子集，TCP/IP协议包括：http、tftp、dns、ip、icmp、arp、rarp等。

   HTTP协议是无连接、无状态的协议。
	无连接的意思是：建立一次连接只能发送一次请求和返回一次响应。
	无状态的意思是：发送完请求后不记录任何东西，无法对之前发生过的请求和响应的状态进行管理

   结构：
     应用层：向用户提供应用服务时通信的活动(HTTP协议)
     表示层
     会话层
     传输层：对应上层应用层，提供处于网络连接中的两台计算机之间的数据传输(TCP协议)
     网络层：用来处理网络上流动的数据包(IP)
     数据链路层：用来处理连接网络的硬件部分
     物理层(硬件)

(2)HTTP报文
    报文又分为请求报文和响应报文

    请求报文
        请求行： 请求方法(get post head等) + 请求资源 url + 请求协议版本

        请求头： referer 用户从何点击过来的
                Content-Type application/x-www-form-urlencoded
                Host 端口
                Content-Length 报文长度

        请求体： 表单提交的数据等
    
    响应报文
        响应行： 响应协议版本号 + 响应状态码 + 响应状态文字

        响应头： Server  服务器
                Content-Type application/json
                Date  日期
        
        响应体：用户需要的数据

### 5.Get和Post的区别
    
    (1)Get用于获取、查询数据，它不会修改服务器上的数据；Post用来传输，改变数据，Post可以向服务器发送修改请求来修改服务器的数据

    (2)Get将请求数据放在url后，以？拼接；Post把数据放在Http正文中

    (3)Get保密性差，效率高，数据不大于2kb;Post保密性高，效率低，大小不受限制

### 6.ajax跨域

(1)ajax本身是不可以跨域的，通过产生一个script标签来实现跨域，因为script标签的src属性是没有跨域的限制的。

(2)于是把数据放到服务器上，并且数据为json形式（因为js可以轻松处理json数据）

(3)定义好能处理跨域获取数据的函数，如 function doJSON（data）{ //可以让数据为我调用的步骤 } (名字可随便)

(4)用src获取数据的时候以？添加一个参数cb='doJSON' 

(5)后台服务端看到cb='doJSON'后，会把数据作为参数传入doJSON()中，再一并返回给客户端


### 7.cookie

Cookie是由服务器端生成，发送给User-Agent（一般是浏览器），浏览器会将Cookie以key/value保存到某个目录下的文本文件内，下次请求同一网站时就发送该Cookie给服务器（前提是浏览器设置为启用cookie）。

Cookie就是一个小型文件，有个数和大小的限制，大小一般是4k，本地可以更改。

Cookie 具有保质期，满足同源策略，可以设置document.domain = 'xxx.com'，使不同源域名之间共用cookie。

封装cookie的使用(增、删、查)
    
    var manageCookie = {
        setCookie: function (key, value, date) {
            var oDate = new Date();
            oDate.setDate(oDate.getDate() + date);
            document.cookie = key + '=' + value + ';expires=' + oDate;
            return this;
        },
        removeCookie: function (key) {
            this.setCookie(key, '', -1);
            return this;
        },
        getCookie: function (key, callback) {
            var cookieStr = document.cookie,
                cookieArr = cookieStr.split(';'),
                len = cookieArr.length;
            for (var i = 0;i < len;i++) {
                var cookieItem = cookieArr[i].split('=');
                if (cookieItem[0] == key) {
                    callback(cookieItem[1]);
                    break;
                }
            }
            return this;
        }
    };

### 8.iframe

(1)iframe就是一个标签dom元素,可以一个网页里嵌入另一个网页。

(2)iframes 阻塞页面加载
    window.onload 事件需要在所有 iframe 加载完毕后(包含里面的元素)才会触发。在 Safari 和 Chrome 里，
    通过 JavaScript 动态设置 iframe 的 src 可以避免这种阻塞情况。

(3)如何获取iframe内的window
    1.document.getElementsByTagName('iframe')[0].contentWindow (子窗口)
    2.document.getElementsById('iFr').contentWindow
    简易写法 
    window.iframeName (iframe的name)

    IE专用
    3.document.iframes[name].contentWindow
    4.document.iframes[i].contentWindow

(4)如何解决iframe受跨域限制问题
    1.document.domain: 解决跨域限制最好的办法
        注：两个域名必须属于同一个基础域名，而且所用的协议，端口都要一致，否则无法跨域
    
    2.window.location.hash: 解决子页面访问父页面数据问题(window.location.href)
        父页面引入子页面时，在iframe的src里添加 # + 数据值，而在子页面里可以访问自己的loaction.hash来获取数据
    
    3.window.name: 解决父页面访问子页面的数据问题
        与父页面不同源的子页面1利用window.name将数据储存在其iframe窗口下，父页面将子页面1更换为与父页面同源的
        子页面2，子页面2可以读取同一iframe窗口下的window.name，再将该数据传给父页面。