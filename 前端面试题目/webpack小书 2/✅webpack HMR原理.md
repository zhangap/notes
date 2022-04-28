热更新又叫热替换，是开发环境常用的一种技术，可以实现局部刷新，不用刷新浏览器而将新变更的模块替换旧的模块。

下面来聊一下常用的webpack-dev-server实现热更新的原理。

1. webpack生成一个Compiler实例，执行Compiler的run方法构建打包文件；
2. webpack-dev-server内置了webpack-dev-middleware和express服务器。webpack-dev-middleware可以把webpack编译生成的文件发送到express服务器，这个服务器就是提供页面访问的；
3. 启动express服务之后，会启动websocket服务，建立本地服务和浏览器的双向通信；
4. webpack构建完成后调用watch方法监听文件变更，当监听到文件变化时重新编译，编译完成后通过websocket通知浏览器更新；
5. 浏览器先发送一个后缀为hot-update.json的文件，获取本次更新的文件名和下一次的热更新的请求文件hash前缀。如果有更新的文件名，就会用JSONP的方式请求后缀为hot-update.js的文件实现增量更新。



参考链接：

1. https://juejin.cn/post/6844904008432222215
1. https://juejin.cn/post/6844904094281236487
1. https://juejin.cn/post/6844903953092591630

