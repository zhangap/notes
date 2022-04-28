#### 水平居中

##### 行内元素

父元素设置 text-align: center；

##### 块级元素

1. 父元素设置：

``` css
display: flex;
justify-content: center;
```

2. 目标元素设置:

``` css
margin: 0 auto;
```

3. 目标元素设置：

```css
position: absolute;
left: 50%;
transform: translate(-50%, 0);
```

4. 目标元素宽度固定时可设置：

```css
position: absolute;
width: calc(var(--target-width) * 1px);
left: 50%;
margin-left: calc(var(--target-width) * -0.5px);
```



#### 垂直居中

1. 目标元素为单行文本，设置父元素：

```css
line-height: calc(var(--target-height) * 1px);
```

2. 设置父元素：

``` css
display: flex;
align-items: center;
```

3. 设置目标元素：

```css
position: absolute;
top: 50%;
transform: translate(0, -50%);
```

4. 目标元素高度固定时可设置：

```css
position: absolute;
width: calc(var(--target-height) * 1px);
top: 50%;
margin-left: calc(var(--target-height) * -0.5px);
```



参考链接：

1. https://juejin.cn/post/6844903474879004680