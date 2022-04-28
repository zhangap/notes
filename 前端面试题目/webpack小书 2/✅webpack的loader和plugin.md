#### 常用的loader

1. file-loader：处理图片和字体，把文件输出到一个文件夹中，在代码中使用相对URL路径去应用；

2. url-loader：与file-loader类似，区别是可设置一个阈值，超过交给file-loader，小于返回base64形式编码；

3. json-loader：加载JSON文件；

4. image-webpack-loader：加载并压缩图片文件；

5. svg-inline-loader：将压缩后的svg注入代码中；

6. less-loader：将LESS代码准换成CSS；

7. sass-loader：将SCSS/SASS代码转换成CSS；

8. postcss-loader：可以配合autoprefixer自动补齐CSS3前缀，做浏览器兼容；

9. css-loader：加载CSS，支持模块化、压缩等特性；

10. style-loader：将CSS代码注入代JS中，通过DOM操作去加载CSS；

11. babel-loader：把ES6转换成ES5；

12. cache-loader：在性能开销较大的loader之前添加，将结果缓存到磁盘里；

13. ts-loader：将TS转换为JS；

14. vue-loader：加载Vue单文件组件；

15. i18n-loader：国际化；

16. thread-loader：多线程打包；

17. eslint-loader：通过eslint坚持js代码；

18. tslint-loader：通过tslint坚持ts代码；

    

#### 手写loader

loader通常是一个函数，接收的参数有3个：

1. content：对于第一个执行的loader为资源的内容，非第一个执行的loader为上一个loader的执行结果;

2. map：可选参数，source-map；

3. meta：可选参数，传递给下一个loader；

   

配置loader路径：

自定义loader可以在webpack.config.js文件中配置resolveLoader.modules，将自定义loader的文件路径添加上去用于查找 。



执行顺序：

loader执行顺序是从后往前的，但是loader的pitch方法刚好相反。先把pitch1、pitch2、pitch3同步代码执行完，再来执行loader3、loader2、loader1，pitch中的异步函数不用等待，异步完了就执行。



主要步骤：

1. 通过loader-utils包里的getOptions方法，获取loader的配置；
2. 通过schema-utils包里的validate方法校验配置是否跟loader校验文件匹配；
3. 校验文件通常是导出一个JSON，包含接收配置的属性以及类型；
4. 校验完成后处理参数，如果是同步loader使用return或者this.callback返回内容，如果是异步loader，使用this.async来获取callback函数，用callback函数返回内容。



#### 常用的plugin

1. html-webpack-plugin：简化HTML文件创建，并自动引入JS；
2. copy-webpack-plugin：复制静态资源；
3. clean-webpack-plugin：目录清理；
4. define-plugin：定义环境变量；
5. mini-css-extract-plugin：分离CSS样式到独立文件；
6. terser-webpack-plugin：支持压缩ES6；
7. uglifyjs-webpack-plugin：压缩JS代码；
8. ignore-plugin：忽略部分文件；
9. speed-measure-webpack-plugin：可以看到每个插件和loader的耗时；
10. webpack-bundle-analyzer：可视化打包文件的体积；



#### 手写plugin

插件都是一个类，所以使用的时候都需要new一个实例。

插件的配置参数都是从构造函数中获取的，类里面必须有一个apply方法，如果是ES5的写法，那就是原型上必须有个apply方法。



在了解步骤之前需要先了解下Tapable类：

Tapable类实现了发布订阅模式。可以简单理解成创建一个实例，将这个实例在不同地方使用可以实现事件通信，类似EventBus。

webpack的构建实际上是调用Compiler实例的run方法，而Compiler类是扩展于Tapable类的。

这就可以理解webpack在运行的生命周期中会广播出许多事件了，而plugin可以监听这些事件，在特定的钩子里添加自定义功能。



主要步骤：

1. compiler暴露了和webpack整个生命周期相关的钩子；

2. compilation暴露了与模块和依赖有关的粒度更小的钩子；

3. 插件需要在其原型上绑定apply方法，才能访问到compiler实例；

4. 传给每个插件的compiler和compilation都是同一个引用；

5. 找到合适的钩子做想做的事，比如copy-webpack-plugin就用到了两个钩子，

   compiler的thisCompilation钩子和compilation的additionalAssets钩子，

   使用tap、tapAsync、tapPromise注册回调函数；

6. 异步事件完成后需要callback才能进入下一个流程。



参考链接：

1. https://juejin.cn/post/6844904094281236487#heading-0