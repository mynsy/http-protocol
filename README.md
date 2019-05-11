## HTTP协议原理+实践

### 导学
- http method
- http status code
- 前端做的最多的关于http 协议的优化就是缓存，对于很多前端来说，做的最多优化可能就是Cache-Control: max-age = 100，对应的静态资源缓存100秒，但是谁知道可以给Cache-Control设置public、private来控制只是在客户端缓存，还是在代理服务器中缓存，must-revalidate缓存过期后必须要到服务端进行验证才能继续使用缓存，通过no-cache,no-store控制是否使用缓存
- 涉及到缓存，就涉及到缓存验证，客户端并不知道服务端数据是否改变，通过last-modified配合if-modified-since验证缓存，还可以通过etag配合if-none-match验证
- 缓存是web服务对性能提升最大的一关，所以深入理解缓存对前后端都非常重要
- 客户端缓存-代理缓存-缓存验证可用性
- http 协议作为web开发里最基础的内容，所有web开发相关的数据传输、内容传输都是通过http协议的
- http很重要，因为前端所有的静态文件加载、数据加载都要通过http协议
- 例子：输入一个url 打开网页，通过ajax获取json/xml/string数据，img 标签的src也是通过http协议请求加载，

### 更多有意义的头
- Content-Type、Content-Encoding等用来约束数据类型
- Cookie 保持会话信息
- CORS实现跨域并保证安全性限制

### 深入到TCP
- 什么是三次握手
- https 连接的创建过程，以及为什么https就是安全的
- 什么是长连接，为什么需要长连接
- http2的信道复用为什么能提高性能
- http 协议是互联网使用最频繁的协议

### 输入URL按下回车那一刻起，都发生了什么
- startTime 开始 按下回车那一刻起
1. Redirect 跳转
    + redirectStart
    + 为什么一开始就要redirect呢？因为浏览器可能记录了这个地址已经永久跳转到别的地址，所以一开始浏览器就要判断是否需要redirect？以及需要redirect哪里
    + redirectEnd 跳转结束
2. App cache 应用缓存
    + fetchStart fetch开始
    + 看缓存，因为请求的资源可能缓存过了，所以要去查看是否有缓存，如果没有缓存，往下走就要去实际服务器里请求资源
3. DNS DNS查找
    + domaiLookupStart 域名解析开始
    + 因为输入的是域名，域名要对应成IP地址之后才能真正的访问到服务器，所以要去查找域名对应的IP地址，所以这叫DNS解析
    + domainLookupEnd 域名解析结束
4. TCP 创建TCP连接
    + connectStart 开始创建链接
    + 如果请求时https的，就是：secureConnectStart 开始创建安全链接，跟TCP的三次握手不一样，因为中间要有一个保证数据安装传输的过程
    + DNS解析后，有了IP地址，就要去创建TCP链接，创建TCP链接需要经过三次握手之后才能真正的把链接创建起来
    + connectEnd 创建连接结束
5. Request 发送请求
    + requestStart 开始发送请求
    + http链接创建好后，才开始真正的发送http请求的数据包
6. Response 接收响应
    + responseStart 开始接收返回
    + 请求发送的数据包发送后，服务器接收到数据，进行数据操作后返回这个请求想要的内容，返回数据后，这个http请求就完成了
    + responseEnd 结束接收返回