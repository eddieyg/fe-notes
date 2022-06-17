# 04 协议


Tcp
传输层协议

Tcp握手连接：
1. 客户端发送Syn包，进入syn_sent发送状态；
2. 服务器发送ACK确认包，进入syn_recv已接收状态；
3. 客户端再发送ACK确认包，进入ESTABLISHED已连接状态

Tcp关闭连接：
1. 客户端发送释放连接报文；
2. 服务端接收后发送确认报文；
3. 服务端将最后数据发送完后，发送释放连接报文；
4. 客户端接收后发送确认报文，连接关闭


Http
https://juejin.cn/post/6844904100035821575
http 是应用层的超文本传输协议，建立在tcp协议之上，连接简单 没有状态，数据明文传输，传输请求由头部请求报文+body数据组成

队头阻塞问题：
由于http1.1的长连接导致的，长连接就是在并发http请求的时候复用一个tcp连接（因为tcp的握手连接耗时所以为了提效），服务端按照请求顺序响应，那么前面响应的比较久就导致阻塞了后面的请求响应
白话http队头阻塞  https://cloud.tencent.com/developer/article/1509279

请求方法get和post的区别：
get 会被缓存，post 不被 缓存;
get 参数放在URL上、需要ascii编码、长度有限制，post 参数则放在请求体中、数据类型大小没有限制


Http缓存
强制缓存 优先级高于 协商缓存

强制缓存（直接读取缓存）
* cache-control：max-age 设置有效秒数、no-cache 不使用强缓存、no-store 不使用任何缓存、private 只在浏览器端缓存 不在代理CDN缓存、public 可以在代理CDN缓存
* expires：缓存资源的过期时间

协商缓存（请求比对是否变更后 响应并读缓存）
* etag：由响应头返回，为资源文件的hash值
* If-none-match：请求时如果 etag 存在，则将 etag为值在请求头携带
* last-modified：由响应头返回，为资源文件的最后修改时间
* If-modified-since：请求时如果 last-modified 存在，则将last-modified为值在请求头携带

图解 HTTP 缓存 https://juejin.cn/post/6844904153043435533


报文

请求 header
accept 接受的数据类型accept-encoding 接受的编码格式connection 是否长连接user-agent 客户端浏览器信息cache-control 缓存控制cookie 存储信息

响应 header
content-type 内容类型context-encoding 内容编码expires 过期时间last-modified 最后修改时间access-control-allow-origin 允许请求的域access-control-allow-methods 允许请求的方法


Http状态码

100 请求处于中间状态

200 请求成功
* 200：OK，表示从客户端发来的请求在服务器端被正确处理
* 204：No content，表示请求成功，但响应报文主体为空

300 请求重定向
* 301：请求资源永久重定向
* 304：请求的资源未修改过，客户端使用缓存
* 307：临时重定向

400 客户端错误
* 400：请求报文存在语法错误
* 401： 请求要求用户的身份认证
* 403：请求资源访问被拒绝
* 404：找不到请求资源
* 405：禁用请求中的方法

500 服务端错误


Https
在http基础上加了一层SSL加密传输协议，提高传输数据的隐私和完整性

服务端需安装CA证书，建立SSL连接；
客户端发送请求过来后，服务端将证书（包含公钥）传输给客户端；
客户端确认SSL安全等级后，用公钥加密会话密钥，服务端用密钥解密出会话密钥；
然后双方用会话密钥加密通信数据，用哈希算法生成摘要


Http2.0

二进制分帧：将请求头部和请求体转成二进制格式，然后拆成多帧传输


CDN

是为了提高访问速度的内容分发网络

客户端到向DNS服务器解析域名，在GSLB负载均衡系统存在对应域名；
则交由GSLB 根据客户端IP位置就近、节点负载情况分发一个访问节点IP；
再通过访问节点请求已缓存资源，没有缓存或已修改节点则去源服务器请求并缓存起来；



