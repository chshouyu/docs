# 常用

## 正则表达式

### email

    /^(\w{1,15})@(?!-)([a-z0-9]*-?[a-z0-9]*[a-z0-9](?!-)(?:\.(?!-)))+[a-z]{2,4}/;
    
#### 解析：

1. `@`前匹配数字、字母、下划线，长度为1-15位
2. `@`之后不能有`-`，然后域名的每一项为数字、字母、`-`的重复，`-`在每一组域名中最多只能有一个，且不能在组的开头或者末尾，即`123-cd`符合，但`-12`或`12-`或`qq--cd`不符合
3. 最后一项为纯字母，长度为2-4位

示例：

```js

var reg = /^(\w{1,15})@(?!-)([a-z0-9]*-?[a-z0-9]*[a-z0-9](?!-)(?:\.(?!-)))+[a-z]{2,4}$/;

reg.test('chshouyu@126.com') // true
reg.test('chshouyu@126.123.abc.com') // true
reg.test('chshouyu@-126.com') // false
reg.test('chshouyu@126-.com') // false
reg.test('chshouyu@126.12-3.com') // true
reg.test('chshouyu@126.12--3.com') // false
```

### 解析URL参数

    /(?:\?|&)(\w+?)=(\w*)/g
    
操作代码：
    
```js
var url = 'http://www.abc.com/get?name=chen&age=25&test=';
var reg = /(?:\?|&)(\w+?)=(\w*)/g; // 不完善，视情况而定
var r, arr = [];

while (r = reg.exec(url)) {
    arr.push({
        key: r[1],
        value: r[2]
    });
}

// result: [{key: 'name', value: 'chen'}, {key: 'age', value: 25}, {key: 'test', value: ''}]

```