## 移动端基于Vue的开发总结
最近几个月一直在搞移动端的事情，不断地挖坑，不断地填坑；总体来说整个过程还是可以的。

### 框架的选择
我们选择使用了`Vue.js`，因为它相对于`React`或者是`Angular`来说都容易上手，API写得非常友好，功能也非常的丰富；然后选择了与之配套的`Vue-Router`，`Vue-Resource`，`Vuex`等相关的模块。（下面会有说明）

### 前后端的分离
在早前前后端没有分离的时候，哪个流程相当痛苦；前端切好图，写好页面，要拿给后端；然后后端再去渲染，如果有交互的话只能使用`jQuery`或者`Zepto`去做，而且后端一旦有改动，前端也要随着改动。整个流程就是，你动我动。不过一旦我们使用了前后端分离的模式之后，前后端依靠文档进行交流；各不影响，大家都相安无事。

### 后端模拟
现在一般有三种选择：

+ 一种是使用原始`express`搭建的小型后端，优点：比较真实的模拟了后端，任何请求，参数都可以进行处理；缺点：需有一定的`node.js`基础，需要手动的配置路由，而且稍微复杂一点。
+ 一种是使用`node.js`直接读取文件，优点：方便简单，很容易写注释；缺点，需要手动配置路由，返回的数据过多比较杂乱，不能够处理请求的参数等。
+ 一种就是淘友助手现在使用的`express`+`mockjs`结合，优点：书写简单，路由可以自己定义，也可以根据文件名来进行请求，返回的数据量较少，可以真实地模拟后端数据，很方便；缺点：不能够根据路由中的参数进行相应的调整。

我们也有图片的模拟接口，可以根据参数生成不同类型的图片，很方便开发的时候使用。

未来的愿景：尽量逼真的模拟后端数据，能够根据我们请求的不同参数做出不同的反应，返回不同的数据。

**:joy:一个问题**：我们定义的接口常常被后端修改，或者有变动如何让双方都知道，中间的度量如何来做，能不能做一个API的管理工具（市面上已经有了），实时同步；大家都在这里观看，修改维护呢？

### 前端静态文件的托管
很幸运我们前端开发者现在处在一个比较好的时代，各种工具层出不穷，其中好用的工具更是很多。我们在移动端选择使用了`Webpack`，`BrowserSync`，`Babel`，`Nodemon`等等（解释）。大大加快了我们的开发效率。

我们主要的开发目录结构：

```
.
├── src
	├── components
		├── ...
	├── config
		├── ...
	├── http
		├── ...
	├── pages
		├── ...
	├── router
		├── ...
	├── vuex
		├── ...
	├── app.vue
	├── main.js
```

这里主要讲解一下`components`，`page`，`vuex`这三个目录的内容。
首先是`components`，其实我们整个项目都是基于组件的，所以组件的开发非常重要，之前一元购的组件写的就比较随意，可扩展性，可维护性都不是很好；淘友助手的组件基本还是比较规范的，可以举个例子：
![](http://angular.angular-china.org/d317c444-4331-49ed-847d-de9deaa98203.jpg)

```javascript
<!-- author: dreamapple -->
<template>
  <div v-show="show" class="mask dr-flex">
    <!-- popup with title -->
    <div v-show="withTitle" class="popup-with-title">

    </div>
    <!-- popup without title -->
    <div v-show="!withTitle" class="popup-without-title">
      <div class="content">{{content}}</div>
      <div class="btns-container dr-flex">
        <div @click="leftBtnAction" :class="{normal: (0 === btnStyleType) || (2 === btnStyleType),
                                             stress: (1 === btnStyleType) || (3 === btnStyleType)}"
             class="left-btn flex-1">{{leftBtnText}}</div>
        <div @click="rightBtnAction" :class="{normal: (0 === btnStyleType) || (1 === btnStyleType),
                                             stress: (2 === btnStyleType) || (3 === btnStyleType)}"
             class="right-btn flex-1">{{rightBtnText}}</div>
      </div>
    </div>
  </div>
</template>

<script>
  'use strict';
  export default {
    props: {
      /*
       * withTitle: 弹窗是否带标题,默认带标题
       * 0: 表示不带标题
       * 1: 表示带标题
       * */
      withTitle: {
        type: Boolean,
        default: true
      },
      /*
       * content 表示弹窗的内容部分
       * */
      content: {
        type: String
      },
      /*
       * 左边按钮的文字
       * */
      leftBtnText: {
        type: String
      },
      /*
       * 右边按钮的文字
       * */
      rightBtnText: {
        type: String
      },
      /*
       * 按钮的样式种类
       * 0:都是normal 1:左边stress右边normal
       * 2:左边normal右边stress 3:都是stress
       * */
      btnStyleType: {
        type: Number,
        default: 0
      },
      /*
       * 是否显示弹窗
       * */
      show: {
        type: Boolean,
        default: false
      }
    },
    methods: {
      leftBtnAction() {
        this.show = false;
        this.$dispatch('left-btn-action');
      },
      rightBtnAction() {
        this.show = false;
        this.$dispatch('right-btn-action');
      }
    }
  }
</script>

<style scoped>
  /* 遮罩层 */
  .mask {
    position: fixed;
    background-color: rgba(0, 0, 0, .5);
    width: 100%;
    height: 100%;
    top: 0;
    z-index: 200;
    flex-direction: column;
    justify-content: center;
  }
  /* 不带标题的弹窗 */
  .popup-without-title {
    display: block;
    margin: auto 4rem;
    padding: 0 1.5rem 2.4rem;
    border-radius: .5rem;
    background-color: #fff;
  }
  .popup-without-title .content {
    display: block;
    margin: 3.8rem auto;
    text-align: center;
    color: #000;
    font-size: 1.6rem;
  }
  .popup-without-title .btns-container {
    text-align: center;
    font-size: 1.4rem;
  }
  .popup-without-title .btns-container .left-btn,
  .popup-without-title .btns-container .right-btn {
    height: 3.7rem;
    line-height: 3.7rem;
    border-radius: .5rem;
  }
  .popup-without-title .btns-container .right-btn {
    margin-left: 1rem;
  }
  .popup-without-title .btns-container .stress {
    background-color: #ff4a4a;
    color: #fff;
  }
  .popup-without-title .btns-container .normal {
    background-color: #f8f8f8;
    color: #898989;
  }
  /* 带标题的弹窗 */
</style>

```
更高级的还有关于内容分发的内容，不过那个会有一些问题，样式的问题有待研究。

下一个是关于页面的的开发：

``` javascript
<!-- author: dreamapple -->
<template>
  <!-- 游戏图片介绍 -->
  <div :style="gameShowStyle" class="game-show flex">
    <div class="game-show__img-container">
      <img :src="pd.gameIcon" class="game-show__img">
    </div>
    <div class="game-show__name">{{pd.gameName}}</div>
  </div>
  <!-- 游戏的图片介绍 -->
  <slider :slider-style="sliderStyle" :pages.sync="carouselList" :sliderinit="sliderinit">
    <!-- slot  -->
  </slider>

  <!-- 游戏的文字介绍 -->
  <div class="game-text-intro">
    {{pd.gameIntro}}
  </div>

  <!-- 底部按钮 -->
  <common-bottom-btn :btn-text="bottomBtnText" @bottom-btn-action="beginPlay"></common-bottom-btn>
</template>

<script>
  'use strict';
  import Vue from 'vue';
  import Slider from '../../components/common-slider/simple-slider.component';
  import { Swipe, SwipeItem } from 'mint-ui';
  import CommonBottomBtn from '../../components/common-btn/common-bottom-btn.component';
  export default {
    components: {
      CommonBottomBtn,
      Swipe,
      SwipeItem,
      Slider
    },
    data() {
      return {
      		// TODO
        }
      }
    },
    computed: {
      gameShowStyle() {
        return {
          // TODO
        }
      }
    },
    methods: {
      beginPlay() {
        // TODO
      }  
    },
    ready() {
      this.gameInfo();
      $('body').css({'background-color': '#fff'});
    },
    destroyed() {
      $('body').css({'background-color': '#f8f8f8'});
    }
  }
</script>

<style lang="scss" scoped>
  .game-show {
    height: 16.3rem;
    flex-direction: column;
    align-items: center;
    background-size: 100% 100%;
    background-position: center;
    background-repeat: no-repeat;

    &__img-container {
      width: 8.8rem;
      height: 8.8rem;
      box-sizing: border-box;
      border: .4rem solid #cdd2d7;
      border-radius: 50%;
      margin-top: 2.4rem;
    }

    &__img {
      display: block;
      width: 100%;
      height: 100%;
      border-radius: 50%;
    }

    &__name {
      margin-top: 1.1rem;
      font-size: 1.6rem;
      text-align: center;
    }
  }

  .game-text-intro {
    margin: 0 1.6rem;
    border-top: 1px solid #dfdfdf;
    color: #898989;
    font-size: 1.4rem;
    line-height: 2.2rem;
    padding-top: 1rem;
    padding-bottom: 1rem;
    text-indent: 1rem;
  }
</style>

```
**@一个问题：**1.关于规范 代码的*注释*、*命名?*风格；2.两个简要的规范，[front-end-style-guide](https://github.com/XJFE/front-end-style-guide)

最后一个就是`Vuex`，在一元购上只有个别地方使用了；使用Vuex的好处很多，推荐在多个页面的交互中使用，尤其是好几个页面共用同一个变量的时候。[vuex](https://github.com/vuejs/vuex/tree/1.0/docs/zh-cn)，而且还有`vue-devtool`的支持，实时监测各种状态和数据，还可以改变任意被监测的状态，非常适合调试；当然它还有许多高大上的东西，后面我们会一步一步的学习起来的。



+ 组件化
+ 模块化
+ 不足与改进
+ 一元购不规范的地方太多了，一步一步修改
+ 注释的书写
+ 关于分支的问题