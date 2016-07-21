# 第一章 移动端尺寸

## 尺寸和分辨率
### IOS

    iphone  3GS: 1X  320 x 480点 = 320 x 480像素
    iphone4(s)： 2X  320 x 480点 = 640 x 960像素
    iphone5(s)： 2X  320 x 568点 = 640 x 1136像素
    iphone6(s)： 2X  375 x 667点 = 750 x 1334像素
    iphone6(s)+: 3X  414 x 736点  (理论上1242 x 2208像素 实际像素是 1080 x 1920)
    
    注意：以上尺寸皆为像素尺寸
    
### Android
    界面尺寸比IOS多很多
    例如:480 x 800 720 x 1280 1080 x 1920 ... (单位:像素)
    
##界面组成元素(IOS Android)

      1、状态栏：显示信号、运营商、电量
      2、导航栏：显示界面的名称、跳转按钮等
      3、内容区域：应用内容
      4、主菜单栏：顶部导航
  
  ## 像素密度（dpi/ppi）
  
    在Android中，规定以160dpi为基准，1dp=1px。如果密度是320dpi，则1dp=2px。（ppi和dpi是同一个概念，Android比较喜欢使用dpi，IOS比较喜欢使用ppi）。
    
    mdpi [320x480]（市场份额不足5%，新手机不会有这种倍率，屏幕通常都特别小）
    hdpi [480x800、480x854、540x960]（早年的低端机，屏幕在3.5英寸档位；如今的低端机，屏幕在4.7-5.0英寸档位）
    xhdpi [720x1280]（早年的中端机，屏幕在4.7-5.0英寸档位；如今的中低端机，屏幕在5.0-5.5英寸档位）
    xxhdpi [1080x1920]（早年的高端机，如今的中高端机，屏幕通常都在5.0英寸以上）
    xxxhdpi [1440x2560]（极少数2K屏手机，比如Google Nexus 6）
    
    iOS和Android平台都定义了各自的逻辑像素单位。iOS的尺寸单位为pt，Android的尺寸单位为dp，实际是同样的意思
    1x：  1pt=1dp=1px（mdpi、iPhone 3gs）
    1.5x：1pt=1dp=1.5px（hdpi）
    2x：  1pt=1dp=2px（xhdpi、iPhone 4s/5/6/6s）
    3x：  1pt=1dp=3px（xxhdpi、iPhone 6(s) plus）
    4x：  1pt=1dp=4px（xxxhdpi）
  
  
  # 尺寸适配例子

    例子：以IOS下 标准屏幕和Retina屏幕为例，想象一个常规的列表页面，在标准和Retina下的表现对比。
    如果一个列表页在3GS上只能显示4-5行，那么4s（宽高是3GS的两倍）是不是能显示9-10行，而且每行会变得特别宽？？？
    而且两款手机其实是一样大的。如果照这种方式显示，3gs上刚刚好的效果，在4s上就会小到根本看不清字。
    
    实际表现：两者效果是一样的。
    
    原因：Retina屏幕把2x2个像素当1个像素使用（倍率是两倍 2x）
    （不考虑状态栏、导航栏等，假设全屏幕都显示列表）
    3GS 实际像素尺寸是 320 x 480， 如果列表一行高度是64px 那么在3GS下一共可以显示480/64=7.5行
    4s  实际像素尺寸是 320 x 480（2x）=640 * 960， 一行高度（64 2x）=128px 那么4s一共可以显示960/128=7.5行
    
    结论：只要两个屏幕逻辑像素（实际像素/倍率=逻辑像素）相同，它们的显示效果就是相同的。


    在设计和开发过程中，应该尽量使用逻辑像素尺寸来思考界面。
    设计Android应用时，有的设计师喜欢把画布设为1080x1920，有的喜欢设成720x1280,界面元素尺寸就不统一了。
    Android的最小点击区域尺寸是48x48dp，这就意味着在xhdpi的设备上，按钮尺寸至少是96x96px。而在xxhdpi设备上，则是144x144px。
    以iPhone 5s为例，屏幕的分辨率是640x1136，倍率是2。浏览器会认为屏幕的分辨率是320x568，仍然是基准倍率的尺寸。所以在制作页面时，只需要按照基准倍率来就行了。只不过在准备资源图的时候，需要准备2倍大小的图，通过代码把它缩成1倍大小显示，才能保证清晰。
    从市场占有率数据来看，目前最多的是iPhone5/5s的屏幕。倍率为2，逻辑像素320x568（所以设计稿为640 x 1136），以后有可能为iPhone 6的屏幕，倍率为2，逻辑像素375x667 （设计稿为750 x 1334）。不管设计稿如何，开发页面都以基础倍率为准。


# 布局适配
    移动端会使用流式布局、固定宽度、响应式做法、viewport缩放、rem等
    1、流式布局：宽度使用百分比，高度使用px，在不同的屏幕适配器下，宽度会被拉伸，导致比例不协调
    2、固定宽度：易于开发，超出的部分在屏幕两边留白
    3、响应式：用media query，维护困难
    4、viewport缩放：天猫的webapp的首页以320宽度为基准，进行缩放，最大缩放为320*1.3 = 416，基本缩放到416都就可以兼容iphone6 plus的屏幕了，这个方法简单粗暴，又高效。但是缩放后图片存在变糊的问题
    5、rem：目前大部分公司主流的一些web app的适配解决方案
    
    常用布局Flex（弹性布局）
    参考资料：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool
    
    概念：采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。
    容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。
![](flex.png)

## 容器的属性
    1、主轴的方向（即项目的排列方向）
    flex-direction: row(默认) | row-reverse | column | column-reverse;
    row（默认值）：   主轴为水平方向，起点在左端。
    row-reverse：    主轴为水平方向，起点在右端。
    column：         主轴为垂直方向，起点在上沿。
    column-reverse： 主轴为垂直方向，起点在下沿。
    
    2、默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。
    flex-wrap： nowrap | wrap | wrap-reverse;
    nowrap（默认）：不换行
    wrap：         换行，第一行在上方。
    wrap-reverse： 换行，第一行在下方
    
    3、（1+2）flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap
    flex-flow：row nowrap
    
    4、justify-content属性定义了项目在主轴上的对齐方式。
    justify-content: flex-start | flex-end | center | space-between | space-around;
    flex-start：左对齐 （默认值）
    flex-end：  右对齐
    center：    居中
    space-between：两端对齐，项目之间的间隔都相等。
    space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
    
    5、align-items属性定义项目在交叉轴上如何对齐
    align-items: flex-start | flex-end | center | baseline | stretch;
    flex-start：交叉轴的起点对齐。
    flex-end：交叉轴的终点对齐。
    center：交叉轴的中点对齐。
    baseline: 项目的第一行文字的基线对齐。
    stretch：（默认值）如果项目未设置高度或设为auto，将占满整个容器的高度。
    
    6、align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
    flex-start：与交叉轴的起点对齐。
    flex-end：与交叉轴的终点对齐。
    center：与交叉轴的中点对齐。
    space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
    space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
    stretch：（默认值）轴线占满整个交叉轴。

## 项目属性
    1、order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。
    order: -1 | 0 | 1 | ...
    
    2、flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
    flex-grow: <number>; /* default 0 */
    
    3、flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
    flex-shrink: <number>; /* default 1 */
    
    4、flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
    flex-basis: <length> | auto; /* default auto */
    
    5、（2+3+4）flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
    
    6、align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
    

# 字体适配

    常用：rem（相对于根元素的字体大小的单位）、em（相对于父元素的字体大小单位）、px（绝对单位）

    
### em
    1em = 16px 那么1.2rem = 19.2px，但是在用px是不带小数的
    所以为了使换算变得简单 设置body: { font-size: 62.5%; } 那么 1em = 10px;
    

# 第二章 移动端常用meta标签

## 1、视图窗口（可视区域）

<!-- 为移动设备添加 viewport -->
    <meta name="viewport" content="initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no">
<!-- `width=device-width` 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边 http://bigc.at/ios-webapp-viewport-meta.orz -->

    *说明：
    width          - viewport的宽度（范围从 200 到 10,000，默认为 980 像素）
    height         – viewport的高度（范围从 223 到 10,000 ）
    initial-scale  - 初始的缩放比例（范围从 0 到 10）
    minimum-scale  - 允许用户缩放到的最小比例
    maximum-scale  – 允许用户缩放到的最大比例
    user-scalable  – 用户是否可以手动缩放 （yes/no）

    *设置说明：
    width=device-width(强制让文档与设备的宽度保持 1:1)

    *区别：
    PC：除去所有工具栏、状态栏、滚动条等等之后用于展示网页的区域（真正有效的区域）


## 2、忽略页面中的数字识别为电话号码（默认识别）

    <meta content="telephone=no" name="format-detection">
    说明：
    一般情况下，IOS和Android系统都会默认某长度内的数字为电话号码


## 3、浏览器相关

### a、IOS中Safari允许全屏浏览（网站开启对webapp程序的支持，默认不支持）

    <meta content="yes" name="apple-mobile-web-app-capable">

### b、IOS中Safari顶端状态条样式（改变顶部状态条的颜色）

    <meta content="black" name="apple-mobile-web-app-status-bar-style">

    说明：
    默认值为 default（白色），可以定为 black（黑色）和 black-translucent（灰色半透明）

    注意：
    若值为“black-translucent”将会占据页面位置，浮在页面上方（会覆盖页面20px高度，Retina屏幕为40px）

### c、IOS中Safari设置保存到桌面图标
    <link rel="apple-touch-icon" href="custom_icon.png">
    说明：
    IOS中Safari特有的meta，保存某个页面到桌面时使用这张图作为桌面图标

    图片尺寸默认设定为 57*57（px）或者 Retina 可以定为 114*114（px），iPad 尺寸为 72*72（px）

## 拓展meta标签
    <!-- 声明文档使用的字符编码 -->
    <meta charset='utf-8'>
    <!-- 优先使用 IE 最新版本和 Chrome -->
    <meta http-equiv="X-UA-Compatible"content="IE=edge,chrome=1"/>
    <!-- 页面描述 -->
    <meta name="description" content="不超过150个字符"/>
    <!-- 页面关键词 -->
    <meta name="keywords" content=""/>
    <!-- 网页作者 -->
    <meta name="author" content="name, email@gmail.com"/>
    <!-- 搜索引擎抓取 -->
    <meta name="robots" content="index,follow"/>
    <!-- 为移动设备添加 viewport -->
    <meta name="viewport" content="initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no">
    <!-- `width=device-width` 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边 http://bigc.at/ios-webapp-viewport-meta.orz -->
 
    <!-- iOS 设备特有 -->
    <!-- 添加到主屏后的标题（iOS 6 新增） -->
    <meta name="apple-mobile-web-app-title" content="标题">
    
    <!-- 是否启用 WebApp 全屏模式，删除苹果默认的工具栏和菜单栏 -->
    <meta name="apple-mobile-web-app-capable" content="yes"/>
    
   <!-- 添加智能 App 广告条 Smart App Banner（iOS 6+ Safari） -->
    <meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL">
    
    <!-- 设置苹果工具栏颜色 -->
    <meta name="apple-mobile-web-app-status-bar-style" content="black"/>
    
    <!-- 忽略页面中的数字识别为电话，忽略email识别 -->
    <meta name="format-detection" content="telphone=no, email=no"/>
    
    <!-- 启用360浏览器的极速模式(webkit) -->
    <meta name="renderer" content="webkit">
    
    <!-- 避免IE使用兼容模式 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    
    <!-- 不让百度转码 -->
    <meta http-equiv="Cache-Control" content="no-siteapp" />
    
    <!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->
    <meta name="HandheldFriendly" content="true">
    <!-- 微软的老式浏览器 -->
    <meta name="MobileOptimized" content="320">
    <!-- uc强制竖屏 -->
    <meta name="screen-orientation" content="portrait">
    <!-- QQ强制竖屏 -->
    <meta name="x5-orientation" content="portrait">
    <!-- UC强制全屏 -->
    <meta name="full-screen" content="yes">
    <!-- QQ强制全屏 -->
    <meta name="x5-fullscreen" content="true">
    <!-- UC应用模式 -->
    <meta name="browsermode" content="application">
    <!-- QQ应用模式 -->
    <meta name="x5-page-mode" content="app">
    <!-- windows phone 点击无高光 -->
    <meta name="msapplication-tap-highlight" content="no">
    
    <!-- iOS 图标 begin -->
    <!-- iPhone 和 iTouch，默认 57x57像素，必须 -->
    <link rel="apple-touch-icon-precomposed" href="/apple-touch-icon-57x57-precomposed.png"/>
    <!-- Retina iPhone 和 Retina iTouch，114x114像素，可选，但推荐有-->
    <link rel="apple-touch-icon-precomposed" sizes="114x114" href="/apple-touch-icon-114x114-precomposed.png"/>
    <!-- Retina iPad，144x144像素，可选，可选，但推荐有-->
    <link rel="apple-touch-icon-precomposed" sizes="144x144" href="/apple-touch-icon-144x144-precomposed.png"/>
    <!-- iOS 图标 end -->
 
    <!-- iOS 启动画面 begin -->
    <!-- iPad 竖屏 768 x 1004（标准分辨率） -->
    <link rel="apple-touch-startup-image" sizes="768x1004" href="/splash-screen-768x1004.png"/>
    <!-- iPad 横屏 1024x748（标准分辨率） -->
    <link rel="apple-touch-startup-image" sizes="1024x748" href="/Default-Portrait-1024x748.png"/>
    
    <!-- iPad 竖屏 1536x2008（Retina） -->
    <link rel="apple-touch-startup-image" sizes="1536x2008" href="/splash-screen-1536x2008.png"/>
    <!-- iPad 横屏 2048x1496（Retina） -->
    <link rel="apple-touch-startup-image" sizes="2048x1496" href="/splash-screen-2048x1496.png"/>
    
    <!-- iPhone/iPod Touch 竖屏 320x480 (标准分辨率) -->
    <link rel="apple-touch-startup-image" href="/splash-screen-320x480.png"/>
    <!-- iPhone/iPod Touch 竖屏 640x960 (Retina) -->
    <link rel="apple-touch-startup-image" sizes="640x960" href="/splash-screen-640x960.png"/>
    <!-- iPhone 5/iPod Touch 5 竖屏 640x1136 (Retina) -->
    <link rel="apple-touch-startup-image" sizes="640x1136" href="/splash-screen-640x1136.png"/>
    <!-- iOS 启动画面 end -->
 
    <!-- iOS 设备 end -->
    <meta name="msapplication-TileColor" content="#000"/>
    <!-- Windows 8 磁贴颜色 -->
    <meta name="msapplication-TileImage" content="icon.png"/>
    <!-- Windows 8 磁贴图标 -->
 
    <link rel="alternate" type="application/rss+xml" title="RSS" href="/rss.xml"/>
    <!-- 添加 RSS 订阅 -->
    <link rel="shortcut icon" type="image/ico" href="/favicon.ico"/>
    <!-- 添加 favicon icon -->

    <!-- sns 社交标签 begin -->
    <!-- 参考微博API -->
    <meta property="og:type" content="类型" />
    <meta property="og:url" content="URL地址" />
    <meta property="og:title" content="标题" />
    <meta property="og:image" content="图片" />
    <meta property="og:description" content="描述" />
    <!-- sns 社交标签 end -->
 
    <title>标题</title>