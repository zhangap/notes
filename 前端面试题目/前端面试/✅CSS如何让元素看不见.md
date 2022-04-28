#### 常用的几种隐藏元素的方法：

1. display：none 真正的隐藏，不占空间，没有事件响应；
2. visibility: hidden 视觉上隐藏，占空间，没有事件响应；
3. opacity：0 视觉上隐藏，占空间，有事件响应；
4. position：absolute/fixed，设置z-index或者定位到可视区之外，脱离文档流，视觉上隐藏；
4. transform: scale(0, 0) 视觉上隐藏，占空间，没有事件响应。



参考链接：

1. https://juejin.cn/post/6844903456545701901