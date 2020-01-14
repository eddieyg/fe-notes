# 混合APP开发
h5内嵌在通过APP的webview

## H5与App通信方式
- 假跳转链接拦截
- 弹窗拦截
- js上下文注入

### 假跳转链接拦截
- `window.location.href = abc://doamin/path?param=123` 同时多个使用location.href通信，除了最后一个，其他的通信都会丢失
