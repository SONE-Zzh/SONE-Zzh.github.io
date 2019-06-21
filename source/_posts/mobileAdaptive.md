---
title: mobileAdaptive
date: 2019-06-20 14:57:03
tags: 
- 前端
- mobile
top: 10
---
### 视口 viewport

meta viewport 标签首先是由苹果公司在其safari浏览器中引入的，目的就是解决移动设备的viewport问题。后来安卓以及各大浏览器厂商也都纷纷效仿，引入对meta viewport的支持，事实也证明这个东西还是非常有用的。
<!-- more -->
通俗的讲，移动设备上的viewport就是设备的屏幕上能用来显示我们的网页的那一块区域，在具体一点，就是浏览器上(也可能是一个app中的webview)用来显示网页的那部分区域，但viewport又不局限于浏览器可视区域的大小，它可能比浏览器的可视区域要大，也可能比浏览器的可视区域要小。在默认情况下，一般来讲，移动设备上的viewport都是要大于浏览器可视区域的，这是因为考虑到移动设备的分辨率相对于桌面电脑来说都比较小，所以为了能在移动设备上正常显示那些传统的为桌面浏览器设计的网站，移动设备上的浏览器都会把自己默认的viewport设为980px或1024px（也可能是其它值，这个是由设备自己决定的），但带来的后果就是浏览器会出现横向滚动条，因为浏览器可视区域的宽度是比这个默认的viewport的宽度要小的。

在苹果的规范中，meta viewport 有6个属性(暂且把content中的那些东西称为一个个属性和值)，如下：

| width         | 设置**\*layout viewport***  的宽度，为一个正整数，或字符串"width-device" |
| ------------- | ------------------------------------------------------------ |
| initial-scale | 设置页面的初始缩放值，为一个数字，可以带小数                 |
| minimum-scale | 允许用户的最小缩放值，为一个数字，可以带小数                 |
| maximum-scale | 允许用户的最大缩放值，为一个数字，可以带小数                 |
| height        | 设置layout viewport  的高度，这个属性对我们并不重要，很少使用 |
| user-scalable | 是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许 |

这些属性可以同时使用，也可以单独使用或混合使用，多个属性同时使用时用逗号隔开就行了。

JavaScript大神Peter-Paul Koch(彼得-保罗 科赫) 将viewport分为了三个概念：

- `visual viewport` 可见视口：屏幕宽度（window. innerWidth/Height）
- `layout viewport` 布局视口： DOM宽度（document. documentElement. clientWidth/Height）
- `ideal viewport` 理想适口：使布局视口就是可见视口（(visual viewport) = (layout viewport) * scale)

**设置理想视口：**把默认的layout viewport的宽度设为移动设备的屏幕宽度，得到理想视口`(ideal viewport)`:

通过width=device-width，所有浏览器都能把当前的viewport宽度变成ideal viewport的宽度，但要注意的是，在iphone和ipad上，无论是竖屏还是横屏，宽度都是竖屏时ideal viewport的宽度。

initial-scale=1 也能把当前的viewport宽度变成 ideal viewport 的宽度，但这次轮到了windows phone 上的IE 无论是竖屏还是横屏都把宽度设为竖屏时ideal viewport的宽度。

但如果width 和 initial-scale=1同时出现，并且还出现了冲突呢，比如：

```
<meta name="viewport" content="width=400, initial-scale=1">
```

width=400表示把当前viewport的宽度设为400px，initial-scale=1则表示把当前viewport的宽度设为ideal viewport的宽度，那么浏览器到底该服从哪个命令呢？是书写顺序在后面的那个吗？不是。当遇到这种情况时，浏览器会取它们两个中较大的那个值。例如，当width=400，ideal viewport的宽度为320时，取的是400；当width=400， ideal viewport的宽度为480时，取的是ideal viewport的宽度。

最后，最完美的写法应该是，两者都写上去，这样就 initial-scale=1 解决了 iphone、ipad的毛病，width=device-width则解决了IE的毛病：

```
<meta name="viewport" content="width=device-width,initial-scale=1">
```

### 像素 （Pixel）

 在css中我们一般使用px作为单位，在桌面浏览器中css的1个像素往往都是对应着电脑屏幕的1个物理像素，这可能会造成我们的一个错觉，那就是css中的像素就是设备的物理像素。但实际情况却并非如此，css中的像素只是一个抽象的单位，在不同的设备或不同的环境中，css中的1px所代表的设备物理像素是不同的。

在早先的移动设备中，屏幕像素密度都比较低，如iphone3，它的分辨率为320x480，在iphone3上，一个css像素确实是等于一个屏幕物理像素的。后来随着技术的发展，移动设备的屏幕像素密度越来越高，从iphone4开始，苹果公司便推出了所谓的Retina屏，分辨率提高了一倍，变成640x960，但屏幕尺寸却没变化，这就意味着同样大小的屏幕上，像素却多了一倍，这时，一个css像素是等于两个物理像素的。其他品牌的移动设备也是这个道理。

 还有一个因素也会引起css中px的变化，那就是用户缩放。例如，当用户把页面放大一倍，那么css中1px所代表的物理像素也会增加一倍；反之把页面缩小一倍，css中1px所代表的物理像素也会减少一倍。

### 物理像素(physical pixel)

物理像素又被称为设备像素，他是显示设备中一个最微小的物理部件。每个像素可以根据操作系统设置自己的颜色和亮度。所谓的一倍屏、二倍屏(Retina)、三倍屏，指的是设备以多少物理像素来显示一个CSS像素，也就是说，多倍屏以更多更精细的物理像素点来显示一个CSS像素点，在普通屏幕下1个CSS像素对应1个物理像素，而在Retina屏幕下，1个CSS像素对应的却是4个物理像素。关于这个概念，看一张"田"字示意图就会清晰了。

### CSS像素（device-independent pixel）

CSS像素是一个抽像的单位，主要使用在浏览器上，用来精确度量Web页面上的内容。一般情况之下，CSS像素称为与设备无关的像素(device-independent pixel)，简称DIPs。CSS像素顾名思义就是我们写CSS时所用的像素。

### 设备像素比（device pixel ratio）

设备像素比简称为dpr，其定义了物理像素和设备独立像素的对应关系。它的值可以按下面的公式计算得到：

设备像素比 ＝ 物理像素 / 设备独立像素

在Retina屏的iphone上，devicePixelRatio的值为2，也就是说1个css像素相当于2个物理像素。通常所说的二倍屏(retina)的dpr是2, 三倍屏是3。

viewport中的scale和dpr是倒数关系。
获取当前设备的dpr：

- `JavaScript: window.devicePixelRatio`。
- `CSS: -webkit-device-pixel-ratio, -webkit-min-device-pixel-ratio, -webkit-max-device-pixel-ratio`。不同dpr的设备，可根据此做一些样式适配(这里只针对webkit内核的浏览器和webview)。

### 屏幕像素密度PPI(pixel per inch)

屏幕像素密度是指一个设备表面上存在的像素数量，它通常以每英寸有多少像素来计算(PPI)。屏幕像素密度与屏幕尺寸和屏幕分辨率有关，在单一变化条件下，屏幕尺寸越小、分辨率越高，像素密度越大，反之越小。

> 屏幕密度 = 对角线分辨率/屏幕尺寸

### 移动端在像素上会遇到的问题

#### dpr问题

<!-- 
  开启文章资源文件夹功能后，应使用图片标签asset_img来插入图片而不是markdown
  直接写文件名称就能获取到
 -->
{% asset_img FpXnQbkqJ8sqP82mmrIjYBxfliQL.jpg retina vs normal %}


上图中可以看出，对于这样的css样式：

```
width: 2px;
height: 2px;
```

在不同的屏幕上(普通屏幕 vs retina屏幕)，css像素所呈现的大小(物理尺寸)是一致的，不同的是1个css像素所对应的物理像素个数是不一致的。

在普通屏幕下，1个css像素 对应 1个物理像素(`1:1`)。
在retina 屏幕下，1个css像素对应 4个物理像素(`1:4`)。

#### 高清图片失真问题

谈到这里，就得说一下，retina下图片的展示情况？

理论上，1个位图像素对应于1个物理像素，图片才能得到完美清晰的展示。

在普通屏幕下是没有问题的，但是在retina屏幕下就会出现位图像素点不够，从而导致图片模糊的情况。

用一张图来表示：

{% asset_img Fuex59zSiV9pbaJG-s9wg_UpCERP.jpg retina image blurry %}



如上图：对于dpr=2的retina屏幕而言，1个位图像素对应于4个物理像素，由于单个位图像素不可以再进一步分割，所以只能就近取色，从而导致图片模糊(注意上述的几个颜色值)。

所以，对于图片高清问题，比较好的方案就是`两倍图片`(@2x)。

如：200×300(css pixel)img标签，就需要提供400×600的图片。

如此一来，位图像素点个数就是原来的`4`倍，在retina屏幕下，`位图像素点个数`就可以跟`物理像素点个数`形成 `1 : 1`的比例，图片自然就清晰了(这也解释了之前留下的一个问题，为啥视觉稿的画布大小要`×2`？)。

这里就还有另一个问题，如果普通屏幕下，也用了`两倍图片`，会怎样呢？

很明显，在普通屏幕下，200×300(css pixel)img标签，所对应的物理像素个数就是`200×300`个，而`两倍图片`的位图像素个数则是`200×300*4`，所以就出现一个物理像素点对应4个位图像素点，所以它的取色也只能通过一定的算法(显示结果就是一张只有原图像素总数四分之一，我们称这个过程叫做`downsampling`)，肉眼看上去虽然图片不会模糊，但是会觉得图片缺少一些锐利度，或者是有点色差(但还是可以接受的)。

用一张图片来表示：

{% asset_img FsYhT3m0Zq3ce-HLBOOlQfY9W2DD.jpg enter image description here %}

DPI(dots per inch)为打印机每英寸可以喷的墨汁点数，用于印刷行业中度量空间点的密度
PPI(pixels per inch)为屏幕每英寸的像素数量(即在一个对角线长度为1英寸的正方形内所拥有的像素数)，用于度量计算机显示屏上像素的密度
目前PPI(主要是iOS)和DPI(比如在Android中)都会用在计算机显示设备的参数描述中，并且二者的意思是一样的，都是代表像素密度

{% asset_img 12617-20160825102300792-1233880647.png 原创移动端高清、多屏适配方案 %}

demo中，100×100的图片，分别放在100×100，50×50，25×25的img容器中，在retina屏幕下的显示效果。

`条形图`，通过放大镜其实可以看出边界像素点取值的不同：

- 图1，就近取色，色值介于红白之间，偏淡，图片看上去会模糊(可以理解为图片拉伸)。
- 图2，没有就近取色，色值要么是红，要么是白，图片看上去很清晰。
- 图3，就近取色，色值介于红白之间，偏重，图片看上去有色差，缺少锐利度(可以理解为图片挤压)。

`爱字图`，可以通过看文字”爱”来区分图片模糊还是清晰。

#### 1px边框问题

##### retina下，border: 1px问题

这大概是设计师最敏感，最关心的问题了。

首先得说一下，为什么存在retina下，border: 1px这一说？

我们正常的写css，像这样`border: 1px;`，在retina屏幕下，会有什么问题吗？

设计师为何觉得`高清屏`下(右图)这个线条`粗`呢？明明和左右一样的~

还是通过一张图来解释(原谅我再次盗图)：

{% asset_img https://images2015.cnblogs.com/blog/12617/201608/12617-20160825102304198-1559877027.png 原创移动端高清、多屏适配方案 %}

上图中，对于一条`1px`宽的直线，它们在屏幕上的物理尺寸(灰色区域)的确是相同的，不同的其实是屏幕上最小的物理显示单元，即物理像素，所以对于一条直线，iphone5它能显示的最小宽度其实是图中的红线圈出来的灰色区域，用css来表示，理论上说是`0.5px`。

**所以，设计师想要的retina下border: 1px;，其实就是1物理像素宽，对于css而言，可以认为是border: 0.5px;，这是retina下(dpr=2)下能显示的最小单位。**

然而，无奈并不是所有手机浏览器都能识别`border: 0.5px;`，ios7以下，android等其他系统里，0.5px会被当成为0px处理，那么如何实现这`0.5px`呢？

最简单的一个做法就是这样(`元素scale`)：

```
.scale{
    position: relative;
}
.scale:after{
    content:"";
    position: absolute;
    bottom:0px;
    left:0px;
    right:0px;
    border-bottom:1px solid #ddd;
    -webkit-transform:scaleY(.5);
    -webkit-transform-origin:0 0;
}123456789101112131415
```

我们照常写`border-bottom: 1px solid #ddd;`，然后通过`transform: scaleY(.5)`缩小0.5倍来达到`0.5px`的效果，但是这样hack实在是不够通用(如：圆角等)，写起来也麻烦。

### 解决方案

{% asset_img 19.jpg keycode %}

如果是之前，我是这样的做法：

> 不断写媒体查询做兼容，直到PM或者QA满意为止。

这样的方法，存在以下几个问题：

1. 难以适应所有的手机屏幕尺寸，总是会有不兼容的尺寸出现，问题仍然存在，只是尚未被发现。
2. 太累了，非常折磨人。

### 我们要达到的效果

- 直接根据UI的标注视觉稿上面的尺寸进行开发。如标注的是`230px`, 通过函数将其转为`rem`而不用人工计算。
- 在大部分的手机机型上看起来的页面视觉效果都一致。



#### 与UI的配合

**首先，需要和UI小姐姐说一句话：**

> "标注元素的时候请按照`750px * 1334px`为准。"

那么，你将会拿到一张如下的标注图：



{% asset_img 14.jpg img%}

### 【核心】动态计算+rem

到这一步，我们仍然没有解决核心问题：

1. 要自己去将px换算成rem。（可能旁边会放一个计算器）
2. 全尺寸适配。

第一步：假设有三款不同长宽的手机。



{% asset_img 13.jpg img%}

第二步：把手机的宽分为10份，那么上述三款手机的每份宽度是**35px/36px/37px**。并且将`<html>`标签添加不同的`font-size`设置。



{% asset_img 12.jpg img%}

**即：一份分别为35px/36px/37px**

{% asset_img 11.jpg img%}

第三步：根据UI的px标注图计算出相应的rem：



{% asset_img 10.jpg img%}

第四步：rem将转化成不同的px尺寸在不同的手机上呈现：(ps:图中的除法结果算错了）



{% asset_img 9.jpg img%}

**通过这样的方式，即可以在不同尺寸的手机上有相同的展示效果。而最cool的地方，是上述整个过程时自动适配的。开发者只需根据UI标注图无脑写就行了，再也不用挤眉弄眼地对着Chrome Devtools 疯狂调试了。**

### 代码实现

> 把手机的宽分为10份，那么上述三款手机的每份宽度是**35px/36px/37px**。并且将`<html>`标签添加不同的`font-size`设置。

通过JavaScript动态计算出当前的屏幕宽度，切割为10份并将`<html>`的`fontSize`设置为`1份单位宽度`。



{% asset_img 8.jpg key-code%}

```
document.addEventListener('DOMContentLoaded', function(e) {
    document.getElementsByTagName('html')[0].style.fontSize = window.innerWidth / 10 + 'px';
}, false);
```

当初始的 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发，而无需等待样式表、图像和子框架的完成加载。另一个不同的事件 load 应该仅用于检测一个完全加载的页面。 

> 根据UI的px标注图计算出相应的rem

### 绕不开的viewport和dpr

在写这篇博客的开始，我曾试图绕开阐述`viewport`和`dpr`这个抽象的概念，因为上述的内容已经可以从一个维度解决大多数问题了。但是，如果想做得更完美，就必须从另一个维度出发，而这个维度，就是**dpr**。



{% asset_img 6.jpg img%}

**首先，要区分两个概念：**

1. **设备的pixels**
2. **css的pixels**

**有这样一个场景**

一位前端工程师敲出了

```
.box {
    width: 100px;
    height:100px;
}
复制代码
```

那么此时，他的意思是`box`在我们的屏幕中占的实际长宽是`100px`,在他脑中是这样的画面：



{% asset_img 5.jpg img%}

项目上线之后，有一个用户'不怀好意'地使用了放大镜功能将长宽放大了两倍，现在就变成了：



{% asset_img 4.jpg img%}

**你会发现，设备花了200px的长宽来渲染CSS里面定义的100px的长宽，而设备pixels和样式pixels的比值，就是dpr，即Device Pixel Ratio**。

{% asset_img 3.jpg dp%}

我们大家都知道Retina屏（视网膜屏），之所以看起来这么高清，就是因为苹果设备花两个像素来渲染一个像素的物体，那么看起来肯定更为精致。

那么，如果我们能够查询出当前设备的**dpr**，并且做相应的缩放就可以解决这个问题。

举个例子：某些安卓机的`dpr=1`，但是UI做标注图的时候是根据`dpr=2`来做的，就像我们上文的`750px * 1334px`。直接按照`750px * 1334px`写出来的元素将会被放大两倍，**那么我们就使页面缩小两倍**，如何控制呢？

**用viewport简言之，在这里我们使用viewport是为了控制屏幕的缩放。**

{% asset_img 2.jpg viewport %}

```
var dpr = window.devicePixelRatio;
meta.setAttribute('content', 'initial-scale=' + 1/dpr + ', maximum-scale=' + 1/dpr + ', minimum-scale=' + 1/dpr + ', user-scalable=no'); 
// 帮助理解 如果dpr=2，说明写的100px渲染成了200px，所以需要缩小至1/2，即1/dpr
复制代码
```

**另外值得一提的是，UI一般会以750px \* 1334px的标准进行设计，因为这样使得设计稿更加精细。**

这样，我们完成了从`dpr`维度的适配。

### 兼容性代码：

{% asset_img 1.jpg sourceCode %}

```
<script>
  var dpr = window.devicePixelRatio;
  var meta = document.createElement('meta');

  // dpr
  meta.setAttribute('content', 'initial-scale=' + 1/dpr + ', maximum-scale=' + 1/dpr + ', minimum-scale=' + 1/dpr + ', user-scalable=no'); 
  document.getElementsByTagName('head')[0].appendChild(meta);

  // rem
  document.addEventListener('DOMContentLoaded', function (e) {
    document.getElementsByTagName('html')[0].style.fontSize = window.innerWidth / 10 + 'px';
  }, false);
</script>
复制代码
```

> 为了防止全局变量污染或者覆盖他人的变量，请封装成模块再使用。

### 百分比与rem布局

`rem布局和百分比布局感觉差距不大啊，因为在写rem的时候是基于把宽度切为10份后再写的，就像是1rem = 10% = 10vw`一样。但是，如果出现盒子嵌套（这种场景太多了），那么百分比布局就出现问题了，因为其百分比的参考系选择的是父元素，所以我们如果在子盒子里面定义`10%`的宽度，指的是针对`父盒子`的而不是我们想要的针对整个`window.innerWidth`的`10%`。而`vw`的代码可维护性不如上述的这套方案，且兼容性也没有`rem`好（这一点差距不是太大）。

### CSS判断横屏竖屏

**写在同一个CSS中**

```
`@media ``screen` `and (orientation: ``portrait``) {``  ``/*竖屏 css*/``} ``@media ``screen` `and (orientation: ``landscape``) {``  ``/*横屏 css*/``}`
```

**分开写在2个CSS中**
竖屏

```
`<``link` `rel``=``"stylesheet"` `media``=``"all and (orientation:portrait)"` `href``=``"portrait.css"``>`
```

横屏

```
`<``link` `rel``=``"stylesheet"` `media``=``"all and (orientation:landscape)"` `href``=``"landscape.css"``>`
```

### JS判断横屏竖屏

```
`//判断手机横竖屏状态：``window.addEventListener(``"onorientationchange"` `in` `window ? ``"orientationchange"` `: ``"resize"``, ``function``() {``        ``if` `(window.orientation === 180 || window.orientation === 0) {``            ``alert(``'竖屏状态！'``);``        ``}``        ``if` `(window.orientation === 90 || window.orientation === -90 ){``            ``alert(``'横屏状态！'``);``        ``} ``    ``}, ``false``);``//移动端的浏览器一般都支持window.orientation这个参数，通过这个参数可以判断出手机是处在横屏还是竖屏状态。`
```



### 如下是其他解决方案

~~~
https://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html
https://www.cnblogs.com/noobfly/p/6207832.html
http://caibaojian.com/mobile-responsive-example.html
~~~



