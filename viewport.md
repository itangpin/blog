
## viewport

## 两个viewport
visual viewport，layout viewport。visual viewport是可视的区域，也就是屏幕的大小，layout viewport则是整个网页的宽度。

### 默认layout viewport
手机浏览器通过将layout viewport设置为一个较大的值，将自己伪装成一个更大宽度的浏览器。这样为大屏幕开发的网页的排版不会受到影响，但是需要通过拖动来查看完整的网页。

### 自定义 layout viewport
我们也可以通过meta标签手动定义layout viewport的宽度。
````html
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=0.5"/>
````

#### width = device-width
在专门为移动设备适配的网页上，他通常被设置为*跟设备宽度一样*`width=device-width`，这样网页就不会超出屏幕。

#### width = 固定值
或者设置为一个*固定值*，如`width=320`，如果这个固定值小于device-width，他会被浏览器重新设置为device-width。

假设你设置width=320，在宽度大于(小于)320的设备上，页面就会被等比放大(缩小)。这样做的问题是大屏幕并不能显示更多的内容。用REM会更加灵活，可参见阿里无线的解决方案。

### 宽度测定
1.layout viewport:
document.documentElement.clientWidth(clientHeight)

2.visual viewport:
window.innerWidth(innerHeight)


## CSS像素
在PC浏览器上，我们把一个支持响应式的网页放大再放大，你会发现这个网页变成了手机排版。






