使用CSS变量做换肤功能，步骤如下：

1. 在css文件中，指定默认的主题色；

   ```css
   /*定义CSS变量*/
   :root {
     --theme-color: '#ff0';
   }
   /*使用CSS变量*/
   body {
     color: var(--theme-color);
   }
   ```

2. 项目初始化时，通过接口获取主题色，并修改主题色变量值；

   ```javascript
   document.documentElement.style.setProperty('--theme-color', themeColor);
   ```



参考链接：

1. http://www.ruanyifeng.com/blog/2017/05/css-variables.html