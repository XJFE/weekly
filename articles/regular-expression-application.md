# 正则在各种环境中的应用

## 基础语法

文档：
 《[正则表达式30分钟入门教程](http://www.jb51.net/tools/zhengze.html)》
 《[正则表达式 - 教程](http://www.runoob.com/regexp/regexp-tutorial.html)》
 《[MDN RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)》

内容就不复制粘贴了，直接看文档。。


## 来几个实际例子

例如：匹配邮箱，匹配手机，匹配密码 ...

其实我的原则，够用就行，别折腾什么精确匹配所有情况。。

### 比如匹配邮箱，百度上的都是:

``` js
/^[a-zA-Z0-9!#$%&'*+\/=?^_`{|}~-]+(?:\.[a-zA-Z0-9!#$%&'*+\/=?^_`{|}~-]+)*@(?:[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?\.)+[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?$/
/^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(.[a-zA-Z0-9_-])+/
/^(\w)+(\.\w+)*@(\w)+((\.\w{2,3}){1,3})$/
```

其实无非就是 `任意字母下划线点减`@`任意字母下划线点减`.`任意字母`

```js
/^[\w.-]+@[\w.-]+\.[a-z]+$/i
```


### 匹配手机：

```js
/((\d{11})|^((\d{7,8})|(\d{4}|\d{3})-(\d{7,8})|(\d{4}|\d{3})-(\d{7,8})-(\d{4}|\d{3}|\d{2}|\d{1})|(\d{7,8})-(\d{4}|\d{3}|\d{2}|\d{1}))$)/
/^1[3|4|5|8]\d{9}$/
```

甚至我之前也写了一个完全版本：

```js
/^0?(13[0-9]|15[012356789]|18[02356789]|14[57])[0-9]{8}$/
```

根据各种运营商提供的信息写的。
但是运营商真蛋疼，三天两头改号段，而且还不公布最新号段，导致用户验证不了。
最头疼的是小号，这个号段之前是固定的，后来不固定，所以无解了。

淘宝注册页面的手机验证是：

```js
/^(86){0,1}1\d{10}$/
```

所以不管怎么样，够用就行。
但也许你会说，那他瞎填怎么破？  
其实邮箱要发验证邮件，手机要发短信验证，无所谓啊，总比验证不过无法注册好吧。



## js中的正则

js的正则，其实用的最多的就那么2，3个，比如 `test`, `match`, `replace`。  
我看很多人都混用，其实数据量不大的情况，几乎没什么影响，数据稍微大一点，性能差异立马体现。  
我就简单介绍下他们的使用场景吧。

### test 方法

`test` 是正则对象上的方法，还有个 `exec` 也是正则对象的方法，不过我几乎不用，因为麻烦。  
`test` 是测试一个正则是否能匹配一个字符串，哪怕有多个匹配项，也不管，只匹配第一个所以性能好。  
往往用于验证字符串是否满足某个条件，比如表单验证之类的。

但 test 有个小坑，在 g (全局匹配)模式下，如果正则赋值给变量，在多次 test 的情况下，一直 true, false 循环。

```js
var re = /b/g;

console.log( re.test('abc') ); // true
console.log( re.test('abc') ); // false
console.log( re.test('abc') ); // true
console.log( re.test('abc') ); // false
```

需要配合 `lastIndex` 属性重置匹配位置，才能得到正确结果。

```js
var re = /b/g;

console.log( re.test('abc') ); // true
re.lastIndex = 0;
console.log( re.test('abc') ); // true
re.lastIndex = 0;
console.log( re.test('abc') ); // true
re.lastIndex = 0;
console.log( re.test('abc') ); // true
```


### match 方法

这个是最常用的 正则匹配 方法了，因为可以得到所有匹配到的值，但也不方便。  
在匹配子表达式的情况下，我更喜欢用 `replace` 代替 `match`，效果逆天。

```js
'sss111ssss222ssssss333ssss'.match(/\d+/g);
```

提取字符串里的某个值：

```js
var str = 'mobile: 123456'
var re = /mobile:\s*(\d+)/;

var mobile = '';
if (re.test(str)) {
  mobile = str.match(re)[1];
}

// 或者 
var m;
if (m = str.match(re)) {
  mobile = m[1];
}

// 或者
if (re.test(str)) {
  mobile = RegExp.$1
}

// 直接
mobile = (str.match(re) || ['']).pop();
```


其他应用例子一下子想不起来。。


### replace 方法

这是字符串的替换方法，很多人只知道他支持正则表达式替换，但不知道他支持替换符。

将字符串中的数字+1

```js
var str = 'sss11ssss22ssss33ssssss';

str = str.replace(/\d+/g, d => ++d);
```

日期格式化

```js
var str = `
ssss: 2016-1-1 - 2016-10-1
ssss: 2016-9-9 - 2016-11-9
`;

str = str.replace(/(?=\b\d\b)/g, '0');
```

中英文字符长度统计(中文算2个长度)

```js
var str = `xx中文xx`; // 长度应该是算为 8

length = str.replace(/[^\x00-\xff]/g, '**').length;
```

其他应用例子一下子想不起来。。



## 编辑器中的 **正则**

其实编辑器现在都支持正则搜索的，但我最喜欢st的方式。
我现在都不用什么正则工具了，直接 st 下写测试，效果相当风骚。

例如写 密码匹配，用 ST 就分分钟搞定。

要求：

1. 字母开头
1. 只能是 数字+字母+下划线 的组合
1. 6-16位
1. 不能纯字母

下面是测试用例：

```
匹配
a12345
a1a1a1
abcde1234567890

不匹配
abcdef
123456
a1234
1a1a1a
abcde12345678901
a-bcdef
```

看风骚的操作。。。

``` js
/^(?![a-z]+$)[a-f]\w{5-15}$/i
```

内容统计，整理，看操作。。


## 命令行下的 正则

基于 grep 的简单命令，win 下的 git hash 好像也支持。。

匹配日志中所有 4xx 错误的日志导入到 4xx.log 文件中

``` sh
$ grep '" 4[0-9]* [0-9]* "' xxx.log > 4xx.log
```
