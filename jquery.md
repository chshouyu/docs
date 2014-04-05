# jquery其他方法备注

## core

### $(selector, context)

可以为选择器指定一个context，从指定的上下文处开始查找

### get([index])

直接得到指定位置的dom元素(以前貌似都是`$('.class')[0]`)

## selector

### parent > child

选择parent下的直接子元素

### prev + next

选择紧跟在prev后的元素

### prev ~ siblings

选择prev之后的所有兄弟元素

### :header

选择h1,h2之类的元素

### contains(text)

匹配包含指定文本的元素

### :empty

匹配空元素

### :has(selector) / :parent

匹配含有子元素的元素

### :only-child

如果某个元素是父元素中唯一的子元素，那将会被匹配

## filter

### map

将一组元素转换成其他数组（不论是否是元素数组）

```html
<p><b>Values: </b></p>
<form>
  <input type="text" name="name" value="John"/>
  <input type="text" name="password" value="password"/>
  <input type="text" name="url" value="http://ejohn.org/"/>
</form>
```

```javascript
$("p").append($("input").map(function() {
    return $(this).val();
}).get().join(", "));

// [ <p>John, password, http://ejohn.org/</p> ]
```

### slice

和原生数组一样(原来jQuery元素有jQuery的方法，汗。。)

以前都是`Array.prototype.slice.call($('.class'))`

## tool

### $.grep

类似underscore的_.filter

### $.makeArray

将类数组的对象转为数组

实际中此函数在 jQuery 中将自动使用而无需特意转换。

相当于`Array.prototype.slice.call`

### $.map

类似underscore的_.map

### $.inArray(value, array, [fromIndex])

类似underscore或者ES5的array.indexOf

### $.merge(first, second)

合并两个数组

### $.unique

删除数组中重复元素。只处理删除DOM元素数组，而不能处理字符串或者数字数组。

### $.proxy

类似underscore的_.bind方法，绑定指定的上下文到一个函数

```js
var obj = {
    name: "John",
    test: function() {
        alert(this.name);
        $("#test").unbind("click", obj.test);
    }
};
```

#### 用法1：

> $.proxy(obj, funcName)
  
funcName是存在于obj中的一个函数

```js
$("#test").click($.proxy(obj, "test"));
```

#### 用法2：

> $.proxy(funcName, obj)
  
绑定funcName到obj

```js
$("#test").click($.proxy(obj.test, obj));
```

#### 实现

```js
var bind = function(func, obj) {
  return function() {
    func.apply(obj, arguments);
  };
};
```

### $.contains

一个DOM节点是否包含另一个DOM节点。

```js
$.contains(document.documentElement, document.body); // true
$.contains(document.body, document.documentElement); // false
```

### $.isWindow

测试对象是否是窗口（有可能是Frame）。

### $.type(obj)

检测obj的数据类型。



