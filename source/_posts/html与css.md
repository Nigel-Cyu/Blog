---
title: html与css
date: 2018-02-20 20:28:52
tags:
---
## HTML笔记
1.html 全称 Hypertext  Markup  Language  超文本标记语言 ，并不是编程语言，而是标记语言。

2.浏览器内核（渲染引擎）：负责对网页语法的解释并渲染页面
	
    IE(微软)       trident  开始接口成熟，处于“垄断”地位，不更新，导致现在bug多
	Firefox        Gecko    代码公开，是开源内核。跨平台，可以在Windows、 BSD、Linux 和 Mac OS X 中使用
	Opera          presto  快、兼容性不好 ，现为chrome的blink
	Chrome(谷歌)   先为 webkit 、后为blink
	Safari(苹果)   webkit   代码结构清晰、渲染速度快
  
	360、UC、QQ、2345等国内浏览器(都是自己的皮肤别人的核)      blink
    
    移动端浏览器内核：
	iPhone 和 iPad       webkit
	Android 4.4 之前     webkit
	Android 4.4 之后     chromiun
<!-- more -->
3.chrome V8引擎：
   ① 快速属性访问  
        不会像查字典一样查询属性位置。而是动态创建隐藏类，每次属性的增、删会在原来基础上创建新的隐藏类
   ② 动态机器码生成
        js代码直接生成为机器码
   ③ 高效分代垃圾回收机制

4.部分常用标签

    标题：<h1>,<h2>,<h3>,<h4>,<h5>,<h6>     显示在页面上字体按顺序由大到小    
    段落：<p></p>
    换行：<br/>
    分割线：<hr/>
    引用短的句子：<q></q>   显示在页面中就是将句子加了一个“双引号”，但是不要只为了需要双引号而是用<q>
    引用大块的句子或段落：<blockquote cite=""></blockquote>    显示在页面是整体缩进,但是不要只为了需要缩进而使用<blockquote>,cite属性值是引用的出处
    定义作品标题：<cite></cite>
    粗体：<b></b>
    斜体：<i></i>
    强调：<em></em>            显示为斜体，不要为了使用斜体而是用em
    强调：<strong></strong>    比em更强烈的强调，显示为粗体,不要为了使用粗体而是用strong
    计算机代码：<code></code>
    保留：<pre></pre>          可以保留文档中的空格、换行和TAB
    上标：<sup></sup>
    下标：<sub></sub>
    无意义标签：<div></div>,可作为容器，用来布局；<span></span>,可以用来和css配合改变文字的样式
    实体字符：   
            <      &lt;
            >      &gt;
            &      &amp;
            空格   &nbsp;
            ©      &copy;
    图像：<img src="图像所在路径" alt="" /> 图片占位符 alt 当页面加载不出来图片的时候就会显示alt中的文本；图片提示符 title
    表格：<table></table> 为什么不用：强制下载完成才执行，用户体验差，样式不好加，自己带有样式 tr行 td单元格
    超链接：<a></a>
        <a href="某个网址" target="_blank" title="超链接">超链接</a>，其中target属性有 _self (默认，在当前页面打开链接页面)，_blank (在新的窗口打开连接页面)，_top (在整个窗口中打开被链接文档)
        
        定义锚点    <a id="tips">锚点</a>        // id 或者 name 都可以
                   <a href="#tips">回到锚点</a>
    无序列表：<ul>
                <li></li>
            </ul>
    有序列表：<ol>
                <li></li>
            </ol>
    自定义列表：<dl>
                    <dt>标题</dt>
                    <dd>内容</dd>
                </dl>
                ul ol dl之间可以互相嵌套
    表单：<form action="" method="">      // actioin 表单提交地址; method 表单提交方式 post 或者 get; 定义id属性，可以让表单外      的表单元素用 form 属性关联到此表单中
            <fieldset>                   // 分区
                <legend>分区标题</legend>    
                账号：<input type="text" name="" /><br/>                 
                密码：<input type="password" name="" /><br/>
                技能:<br/>
                <label><input type="radio" name="ht" checked="checked" />html</label>      // 同一组(单选) radio 的 name 值必须相同；checked 默认选中
                <label><input type="radio" name="ht" />css</label>                         // input 外加 label 为了点击字体的时候也可选中radio ;另一种方法是<label> 中的 for 属性和 radio 的 id 属性值相同 
                <label><input type="radio" name="ht" />javascript</label><br/>
                兴趣:<br/>
                <label><input type="checkbox" name="l" value="骑行" />摄影</label>
                <label><input type="checkbox" name="l" value="书法" checked="checked" />蹦迪</label>
                <label><input type="checkbox" name="l" value="旅行" />旅行</label>
            </fieldset>
            城市：
            <select>
                <option>北京</option>
                <option selected="selected">上海</option>       // selected 默认选中
                <option>广州</option>
            </select><br/>
            简介：<br/>
            <textarea cols="30" rows="5"></textarea><br/>
            <input type="submit" name="" />
            <input type="reset" name="" />
            <input type="button" name="" value="按钮" />      // 按钮还是用 <button> 标签好一些 
            <br/>
            <button type="submit">提交</button>
        </form>

            display 禁用； required 必须填写项； placeholder 提示词（显示灰色）；multiple 可多选；min max 最小值、最大值；width 和 height  input属性；
            input 的 type 属性还有其他值，比如，email，url,number, search,range,color,data,aurofocus

5.渲染模式：标准模式和怪异模式
    <!doctype html>严格模式 以最高标准来加载
	没有则为混杂(怪异)模式 向前兼容

	判断：document.compatMode   BackCompat	 怪异模式
						        CSS1Compat  标准模式   w3c标准

	版本：过渡的(Transitional)：允许你继续使用HTML4.01的标识
	      严格的(Strict)：你不能使用任何表现层的标识和属性，例如<br>
	      框架的(Frameset)：针对框架页面设计使用
    
    区别:①盒模型的处理差异:标准CSS盒模型的宽度和高度等于内容区的高度和宽度，不包含内边距和边框，而怪异模式
                         是包含内边距和边框的；
         ②行内元素的垂直对齐:怪异模式下它们会对齐至底部，而标准模式下是基线对齐。

6.href和src
     href： 超文本引用，指向网络链接所在的位置，用来建立当前元素和文档之间的链接 用于link a
     src：  地址来源，src指向的内容会嵌入到文档中当前标签所在的位置 用于img iframe script
     简而言之，src用于替换当前元素；href用于在当前文档和引用资源之间建立联系。

7.web标准：行为、样式、结构相分离

## CSS笔记
1.权重
     !important         无穷大
     行间样式            1000
     id                 100
     CLASS/伪类/属性     10
     标签/伪元素         1
     通配符              0

2.行内元素和块级元素
    块级元素(block)：div, p, form, ul, li, ol, dl, form, address, fieldset, hr, menu, table, h1~h6
    行内元素(inline)：span, a, strong, em, br, img, input, label, select, textarea, cite
    ps:行内元素无法设置宽高，但设置 display:inline-block 后可以。

3.css代码三种引入方式：
	行间样式：使用标签元素的style属性引入css
	内部样式：在style标签下书写css，style标签写入head标签中
	外部样式：①链接式：在link标签下的href属性中写入css文档
	         ②导入式：在style标签下写 @import url("css文档路径")

4.!important 无法提升继承样式的权重，也不会影响就近原则。

5.em是一个字长是相对的单位，而px是像素的意思，是绝对的单位。

6.bfc
    Block Formatting Context 块级格式上下文。BFC就是一种布局方式，在这种布局方式下，盒子们自所在的containing block顶部一个接一个垂直排列，水平方向上撑满整个宽度。
    
    如何触发bfc: ①float属性不为none
                ②overflow不为visible(可以是hidden、scroll、auto)
                ③position为absolute或fixed
                ④display为inline-block、table-cell、table-caption
    
    bfc的作用：(1)解决margin塌陷问题(清除内部浮动)
                对子元素设置浮动后，父元素会发生高度塌陷，也就是父元素的高度变为0(父子间 margin-top 取最大值)。
                解决方法是把父元素变成一个BFC。常用的办法是给父元素设置 overflow:hidden。
              (2)解决margin合并问题
                如果两个div上下排序，给上面div设置margin-bottom，给下面div设置margin-top，则两个margin会
                发生合并现象，合并后的值取较大的那个。解决方法是分别给父元素设置 overflow:hidden，再给其中
                一个div设置 display:inline-block。

7.浮动float
    
    浮动的特点： Ø 块级元素浮动后变成inline-block
                Ø 浮动的元素会脱离标准流，浮动以后的元素会覆盖在标准流的元素之上
                Ø 浮动元素可以看到浮动元素，触发bfc的元素可以看到浮动元素
                Ø 浮动规则：浮动找浮动，不浮动找不浮动
                    浮动找浮动：只有写在同一个结构下面的浮动才会浮动找浮动
                Ø 浮动显示的位置与原本不浮动之前的位置是对应的
                Ø 浮动的重点：同一等级，浮动的元素只会影响下面的元素，不会影响上面的元素！
    
    清除浮动的方法：Ø 在父元素上加:after{
                                    content: "";
                                    display: block;
                                    clear: both;
                                }
                     ps:为了兼容ie6 父元素还要设置 zoom: 1 ,因为ie6版本不支持 overflow：hidden清除浮动
                   Ø 加一个元素并设置 clear：both
                   Ø 父元素上加overflow：hidden
                   Ø 父元素上加position：absolute

8.单行文字打点
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;

9.语义化
具体来说，就是在书写html时，尽量使用具有语义信息的标签，例如header,nav,aside,section等代替那些没有语义信息的标签，例如big,center,strike,font等（完全可以用css来取代的标签）。这样不仅有利于页面DOM的组织，也有利于机器（主要是搜索引擎）的理解。
    
    作用：   (1)去掉样式后页面呈现清晰的结构
            (2)盲人使用读屏器可以更好的阅读
            (3)搜索引擎更好地理解页面，有利于收录
            (4)使团队的项目可以持续的运作和维护

10.定位
(1)static： 静态定位(默认)
    所有的标准流中的元素都是静态定位

(2)relative： 相对定位
    相对于自己原来的位置发生偏移，在页面上仍然占据位置，会覆盖其他内容，用于微调元素、做绝对定位的参考。

(3)absolute： 绝对定位
    绝对定位的参考点：
    
    Ø  如果这个元素没有父元素，相对于body来定位

    Ø  如果绝对定位的元素有父元素，但是父元素没有定位，还是相对于body来定位的

    Ø  如果绝对定位的元素有“父”元素，而且父元素有非static定位，那么这个绝对定位的元素相对“父”元素来定位。
       要听最近的已经定位的祖先元素的，不一定是父亲，可能是爷爷！

    Ø  绝对定位的儿子，无视所参考的父元素的padding

    Ø  绝对定位之后的元素在页面不会占据位置（绝对定位以后的元素会脱离标准流）

(4)fixed：固定定位
    
    Ø  不管页面有多大，固定定位的元素永远是相对于浏览器的边框来的。

    Ø  固定定位的元素也脱离了标准流（不在页面上占据位置）

(5)sticky：粘性定位
    在屏幕范围内(viewport)时该元素的位置不会受到定位影响(设置left、top等属性无效)，但当其移出偏移范围时，定位效果变为fixed,并根据所设置的left、top等属性进行定位。
        注：
			Ø  仍然保留元素原本在文档流中的位置
			Ø  元素固定的相对偏移是相对于离它最近的具有滚动框的祖先元素，如果祖先元素都不可以滚动，那么是相对于
                viewport来计算元素的偏移量
			Ø  只兼容 FireFox 和 iOS 的 Safari

11.伪元素
:first-letter	向文本的第一个字母添加特殊样式
:first-line	    向文本的首行添加特殊样式
:before	        在元素之前添加内容
:after	        在元素之后添加内容

12.伪类
Ø  a:link            给a标签设置没有被访问过的样式

Ø  a:visited         给a标签设置被访问过的样式

Ø  a:hover           给a标签设置鼠标悬停时的样式

Ø  a:active          给a标签设置被点击的样式
    
    注：在使用的时候一定遵守这样的顺序：a:link,a:visited,a:hover,a:active，否则会失效！
        :link、:visited只能用于a标签，:hover、:active 其它标签也可以使用
