简答：

本地存储会更安全一些。

因为客户端在发起请求的时候，会将cookie放在请求头里，一起发送给服务端。



可以往下面几个点聊：

1⃣️ WebStorage

需要注意key和value都是字符串类型，如果存储的value为对象，可以使用JSON.stringify和JSON.parse转换。

* localStorage : 本地缓存
* sessionStorage: 会话缓存

相同点：

都是可以用来做持久化，刷新网页数据不会消失

不同点：

1. 大小：cookie只能存储4k左右的数据，localStorage和localStorage能存5M甚至更多的数据。
2. 有效期：
   * cookie设置的有效期期间。如果不设置有效期，默认为会话型存储。数据存储在浏览器内存里，当关闭浏览器（非关闭标签窗口）时会清空缓存数据；
   * sessionStorage在关闭标签后就会失效；
   * localStorage始终有效，保存在磁盘中，需主动删除才会失效。

3. 作用域：
   * sessionStorage在不同标签tab下同源窗口数据不共享，cookie和localStorage在同源窗口下数据共享；
   * cookie可以设置domain和path，设置当前cookie可以被访问的路径。

4. 事件监听：WebStorage支持对storage数据操作（删/改）的监听，监听事件名为storage。

5. 数据传输：cookie会被放在请求header里，一起发送到服务端，而WebStorage不会。

   

2⃣️ web前端安全

详情见另一篇文章：web安全



参考链接：

1. https://blog.csdn.net/weixin_42614080/article/details/90706499