jQuery将自己注册到全局变量里的方式简化如下。
```javascript
(function(window, factory) {
    factory(window)
}(this, function() {
    return function() {
       //jQuery的调用
    }
}))
```
第二个参数是一个工厂函数，