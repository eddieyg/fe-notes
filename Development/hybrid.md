# 混合APP开发
h5内嵌在APP的webview

## webview内核
`Android`的APP webview默认都是使用手机的内置浏览器内核（APP可更改内核，如腾讯x5），各手机厂商都对系统自带浏览器内核Webkit进行二次开发，所以导致安卓手机之间会有很多差异和兼容问题。  
`Ios`由于系统封闭，所以APP webview都是使用系统自带的Safari内核，较为统一。

## H5与App通信方式
- 假跳转链接拦截（尽量使用`iframe`加载协议链接；避免使用`location.href`，因为当同时跳转多个协议链接通信的时候，除了最后一个，其他的通信都会丢失。）
- 弹窗拦截
- js上下文注入

