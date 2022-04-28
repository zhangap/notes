BFC的中文翻译是块格式化上下文。

BFC是CSS渲染的一部分，是块盒子布局的区域，也是浮动元素与其他元素交互的区域。



#### 怎么创建BFC（怎么创建块格式化上下文区域）

1. 根元素<html>；
2. 使用float属性，只要不为none，如left、right；
3. 使用overflow属性，只要不为visible，如auto、hidden、scroll；
4. 使用display属性，如flex、flow-root、inline-block、table；
5. 使用position属性，如fixed、absolute；
6. ...



#### BFC区域有什么特性

1. 内部的盒子会垂直按顺序排列；
2. 处于同一个BFC的元素会相互影响，可能会出现外边距折叠；
3. BFC具有隔离功能的容器，容器里的子元素和外面的元素互不影响；
4. 计算BFC的高度时，考虑BFC里的所有元素，包含浮动元素；
5. BFC包含所有子元素，但不包含创建了新的BFC的子元素的内部元素；
6. ...



#### 怎么解决BFC导致的外边距重叠

上面块元素的下外边距和下面块元素的上外边距有时候会发生重叠，间距合并取两个外边距的最大值，这种情况就是外边距重叠。

这个也可以说是BFC的一个副作用，因为同一个BFC里的元素会相互影响。

处理外边距重叠的方法，就是将两个元素放置在不同的BFC中，如给下面的元素外层包裹一个div父元素，将父元素创建为一个新的BFC。



#### 怎么解决子元素浮动后父元素高度坍塌

1. 将父元素创建为一个新的BFC，这样计算BFC的高度就可以将浮动元素也包含在内；
2. 给父元素的伪元素，添加clear:both;的样式属性。



参考链接：

1. https://juejin.cn/post/6950082193632788493
1. https://juejin.cn/post/6950082193632788493?utm_source=gold_browser_extension
1. https://juejin.cn/post/6844903495108132877
1. https://juejin.cn/post/6982179919597928485
1. https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context