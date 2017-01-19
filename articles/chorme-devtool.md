# Chrome 调试姿势普及

## 开发者工具应用的境界

### 境界一：辅助开发

看 html 结构，微调 css 样式，检查 ajax 请求等。


### 境界二：问题排查

控制台测试，断点动态调试，network深度分析等

### 境界三：性能调优

性能分析，页面审计，综合评估等。


**一个优秀的 weber 能从开发者工具中看到一切**


## 调试姿势

 《[js调试系列](http://www.cnblogs.com/52cik/p/js-console-acquaintance.html)》
 《[浅谈 jQuery 事件源码定位问题](http://www.cnblogs.com/52cik/p/jquery-source-position.html)》

3,5分钟过下教程，然后看实际演示。


### 快速定位功能代码

例如找到淘宝注册时候的验证手机功能。

全局变量泄露的排查。

``` js
(function () {
  fuckIE = 'hehe';

  for (i=0; i<10; i++) {
    fuckIE += i;
  }
})();
```

函数/方法的功能断点调试。

等等。。。

## network 的数据分析

http://www.cnblogs.com/zhenwen/p/5827925.html


掌握这些就差不多了，能有效的提升开效率。

其他的 Timeline， Profiles， Autits 都是做性能优化的，我们以功能为主，性能都之后考虑。



## node 端调试

1. node-inspector 老牌调试工具了
2. devtool
3. vscode

