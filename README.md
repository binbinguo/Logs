# Logs
开发日记


* [2017年06月30日-晴转阵雨](https://github.com/binbinguo/Logs/blob/master/README.md#2017年06月30日)
* [2017年07月03日-晴](https://github.com/binbinguo/Logs/blob/master/README.md#2017年07月03日)
* [2017年07月04日-晴](https://github.com/binbinguo/Logs/blob/master/README.md#2017年07月04日)


## 2017年06月30日

<span id="#log_20170630"></span>

加入[360]()上海团队后, 前期负责一些广告位模块开发, 要求兼容到IE6. 这对之前并不太注重IE低版本兼容性的我带
来了不小考验, 遂决定将IE低版本兼容性方面的知识再看看，记录一些容易踩坑或容易被网络误导方面的内容。
##### No1. IE盒子模型与W3C标准盒子模型的区别

IE盒子模型和W3C标准盒子模型到底有什么区别？ 网上搜索答案时很多关于这方面的介绍,很全面，如[标准W3C盒子模型和IE盒子模型CSS布局经典盒子模型(转)
](http://www.cnblogs.com/cchyao/archive/2010/07/12/1775846.html)。

但是：太笼统！ 并不是所有的IE盒模型都和W3C标准盒模型不一致。

**key:**
* **IE6-**
* **怪异模式**

具体亲测只有**IE6-**（IE5以下没有亲测）在[怪异模式下](https://www.ibm.com/developerworks/cn/web/1310_shatao_quirks/)的盒模型才与标准盒模型有区别，表现在IE6-盒模型的width、height包括了padding值，盒子的整个宽度、高度分别为
width+margin、height+margin,而标准的盒模型width、height只是content的width、height，不包含padding及margin，对应盒子整体的宽度和高度则需要把padding和margin都加上

**如何避免？**

* 在html头部加上标记:
````html
<!DOCTYPE html>

````
使用标准盒模型，即使是在IE6下依然生效，参照[注意ie6的盒模型](http://www.cnblogs.com/myit/p/4121302.html)

##### No2. ie6不支持CSS :hover 伪类选择器

在**IE6-** 版本对CSS hover 伪类选择器支持的不好，亲测在IE6下给DOM附加的hover样式并不生效，网搜主要解决方法分为两种：

- 引入csshover.htc文件
- 使用js+添加hover类

我对两种都进行了尝试， 第一种效果最好，不过需要提前下载[csshover.htc](http://download.csdn.net/download/crazy_aka/4184929)，具体的使用方式如下：
```html
<!--[if IE 6]>
    <style>
        body {behavior: url("../csshover.htc");}
    </style>
<![endif]-->
```
很简单吧，第二种是结合js实现，大致原理是给所有DOM添加onmouseover和onmouseout事件，动态添加hover类，css中通过#target.hover的形式代替
target:hover

##### No3. ie6不支持png图片透明

在IE6版本中，png图片不再透明，变成了黑底图片，可采用的[解决办法]():

- 下载[DDPngMin.js](http://www.bvbcode.com/cn/e37g9oak-975765)文件
- 需要修改的png图片类名或者ID名

具体解决如下：

````html
<!--[if IE 6]>
    <script src=""js/DDPngMin.js""></script>
    <script>DD_belatedPNG.fix('.ad_img img,#banner_ctr ul');</script>
<![endif]--> 
````

## 2017年07月03日
今天尝试了新的编辑器sublime，之前也是在网上看见很多同猿类推荐使用这款轻量级附文本编辑器，自己也想体验一下，初识感觉还可以，第一感觉资源相对于Intellij这种编辑器来说减少了很多，很轻便。但是随之而来的是我如果要使用，它能提供像Intellij idea那样丰富的功能么？ 答案是它丰富的插件系统基本能解决我大部分诉求。

但是

- 在markdown文件方面却有些不足之处

 有些markdown插件对markdown语法支持的并不好，比如:

````html
<!DOCTYPE html>

````
在markdownonlinepreview插件上始终无法正确显示html块，后来又装了些其他的插件，感觉都不理想，直到遇到它：

- OmniMarkupPreviewer

虽然也遇到了写波折，比如浏览[出现404](http://blog.csdn.net/dream_allday/article/details/52951976)，按照大牛的指导果然有效！

## 2017年07月04日

在调试IE低版本（IE6、IE7）时发现div的**overflow:hidden**属性并不生效

- 解决办法为：
````css
    div {
        ……
        *position:relative;
        ……
    }
````
即[将div的position属性设置成相对定位](http://www.111cn.net/cssdiv/163/42133.htm), 并加上IE hack。
### 进一步深究
- 产生原因--bug

低版本IE（IE6/7)中，当父元素的直接子元素或者下级子元素的样式拥有position:relative属性时，父元素的overflow:hidden属性就会失效。
