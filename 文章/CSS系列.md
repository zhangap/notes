# 一、CSS

## 1、基础知识

1. 新增颜色模式：rgba(0,0,0,0.5)。

2. text-shadow属性向文本设置阴影。

3. 文字描边：在使用时要增加-webkit前缀。

4. 文字排列方向：direction:rtl; unicode-bidi:bidi-override; 全兼容

5. 文字超出边界出现省略号：white-space:nowrap; overflow:hidden; text-overflow:ellipsis

6. 自定义字体：

   ```css
   @font-face {
       font-family: ‘miaov';
       src: url('111-webfont.eot');
       src: url('111-webfont.eot?#iefix') format('embedded-opentype'),
            url('111-webfont.woff') format('woff'),
            url('111-webfont.ttf') format('truetype'),
            url('111-webfont.svg#untitledregular') format('svg');
       font-weight: normal;
       font-style: normal;
    
   }
   转换字体格式生成兼容代码：http://www.fontsquirrel.com/fontface/generator
   ```

   

7. 弹性盒模型

   注意在使用弹性盒模型的时候，父元素必须要加display:box或display:inline-box。

   - box-orient定义盒模型的布局方向
     - horizontal定义水平显示
     - vertical垂直方向
   - box-direction元素排列顺序
     - normal 正序
     - reverse反序
   - 相对于子元素的相关设置
     - box-ordinal-group设置元素的具体位置
     - box-flex定义盒子的弹性空间
     - 子元素的尺寸=盒子的尺寸*子元素的box-flex属性值/所有子元素的box-flex属性值的和。
   - 相对于父元素的设置
     - box-pack对盒子的富余空间进行管理
       - start所有子元素在左侧显示，富裕空间再右侧。
       - end所以子元素在右侧显示，富裕空间再左侧。
       - center所有子元素居中显示。
       - justify富裕空间在所有子元素之间平均分布。
   - 相对于子元素的设置
     - box-align在垂直方向上对元素的位置进行管理
       - start所有子元素聚顶
       - end所有子元素聚低
       - center所有子元素居中

8. 清除浮动的几种方式：

   1. 给父级元素设置合适的高度
   2. 给父级元素设置overflow:hidden
   3. clear:both

9. 对于block元素

   1. padding值很大时（超过元素本身的宽度），一定会影响尺寸大小。
   2. width非auto,padding会影响尺寸
   3. width为auto或者是box-sizing为border-box，同时padding值正常的，不影响尺寸。

10. 对于inline(内敛元素)水平元素

    - 水平padding影响尺寸，垂直padding不影响尺寸，但是会影响背景色（占据空间），padding不支持任何形式的负值

11. 所有浏览器input/textarea输入框内置padding

12. 所有浏览器button按钮都内置了padding

13. 部分浏览器select下拉内置padding，如Firefox，IE8可以设置padding

14. 所有浏览器radio/checkbox单复选框无内置padding

## 2、BFC规定了内部的Block Box如何布局

1. 定义：BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。
2. 定位方案
   - 内部的box会在垂直方向上一个接一个的放置。
   - box垂直方向的距离由margin决定，属于同一个BFC的两个相邻的margin会发生重叠。
   - 每个元素的margin box的左边，与包含块border box的左边相接触。
   - BFC的区域不会与float box重叠。
   - BFC是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。
3. 计算BFC高度时，浮动元素也会参与计算
4. 满足下列条件之一就可以触发BFC：
   - 根元素，即html
   - float的值不为none(默认)
   - overflow的值不为visible（默认）
   - display的值为inline-block/table-cell/table-caption
   - position的值为absolute或fixed

## 3、浏览器是怎么解析CSS的？

- ​	CSS选择器的解析是从右至左解析的。
- 若从左向右匹配，发现不符合规则，需要进行回溯，会损失很多性能。
- 若从右向左匹配，先找到所有的最右侧节点，对于每个节点，向上寻找其父节点直到找到根元素或满足条件的匹配规则，则结束这个分支的遍历。
- 两种匹配的规则的性能差别很大，是因为从右至左的匹配在第一步就筛选了大量不符合条件的最右节点（叶子节点），而从左至右的匹配规则的性能都浪费在失败的查找上面。
- 而在css解析完毕后，需要将解析的结果与DOM Tree的内容以一起进行分析建立一棵 Render Tree，最终用来渲染绘图。
- 在建立renderTree 时（webkit中的attachment过程），浏览器就要为每个DOM tree中的元素根据css的解析结果来确定生成怎样的Render Tree。

## 4、浮动的原理和工作方式，会产生什么影响，要怎么处理？

工作方式：浮动元素脱离文档流，不占据空间。浮动元素碰到包含它边框或者是浮动元素的边框停留。

影响：

1. 浮动会导致父元素无法被撑开，影响与父元素同级的元素
2. 与该浮动元素同级的非浮动元素，如果是块级元素，会移动到元素的下方，而块级元素内部的行内元素会环绕浮动元素。而如果是内联元素则会环绕该浮动元素。
3. 与该父元素同级的浮动元素，对于同一方向的浮动元素（同级），两个元素将会跟在碰到的浮动元素后，而对于不同方向的浮动元素，在宽度足够时，将分别浮动到不同方向，在宽度不够时，将会导致一方换行（换行与HTML书写顺序有关，后边的将会浮动到下一行）。
4. 浮动元素将被视作块级元素。
5. 浮动元素对于其父元素之外的元素，如果是非浮动元素，则相当于忽视该浮动元素，如果是浮动元素，则相当于同级的浮动元素。
6. 常用的清除浮动的方法：设置父元素的高、空标签、overflow、伪元素。

# 二、