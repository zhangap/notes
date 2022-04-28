webpack是一个打包工具，性能优化可以从打包时间和打包体积来考虑。

不根据应用场景谈结论都是耍流氓，下面就从webpack使用环境的维度来聊一聊优化。



* 无论是开发环境和生产环境都可以做的优化：

  1. 多进程打包-之前有HappyPack，现在不维护了。现在比较主流的thread-loader， react脚手架搭建的工程也在用；

  2. 使用缓存-比如babel-loader这种比较耗时的loader，可以配置cacheDirectory开启缓存，也可以使用cache-loader来开启缓存；

  3. DLL分包-将不常更新的第三方库单独打包，在构建的时候将第三方包引入 ，每次打包的内容就会更少，速度会更快；

  4. extelnals-将第三方库在index.html中以CDN的方式引入，在webpack中配置externals，表明用的是外部依赖；

  5. oneOf-找到匹配的loader之后，就不往后找了，节省loader的匹配时间；

  6. 代码分割-将代码按路由或者组件维度分割，实现按需加载、提取公共代码；

     * 入口起点：使用entry配置手动分离代码；
     * 动态导入：通过动态导入模块（如import）分离代码，路由/组件懒加载用的就是这个方式；
     * 配置去重和分离规则：在webpack.config.js文件中，配置optimization.splitChunks，实现公共代码提取和代码分割。

  7. 文件指纹：文件指纹是打包后文件名的后缀，有三种类型：hash-把把不一样，chunkhash-入口一样就一样，contenthash-内容一样就一样。合理使用文件指纹，利用浏览器的缓存机制；

     * JS-chunkhash;
     * CSS-contenthash;
     * 图片-hash。

  8. 缩小打包作用域;

     * 使用include或者exclude指明loader范围；
     * Resolve.modules指明第三方库的绝对路径，减少不必要的查找。

     

* 开发环境可以做的优化：

  1. HMR-模块热替换是开发环境一个很重要的优化手段，当有一个模块发生变化时，只需要重新打包这个模块，而不是打包所有文件；
     * HTML以入口引入的方式实现热替换；
     * CSS在style.loader转换的时候实现了热替换；
     * JS需要手动使用module.hot的方式实现，module.hot.accept的回调中是热替换后会执行的代码。
  2. 代码调试-source-map是一种提供构建后的代码映射到源代码的一种技术，有多种类型可以选择。开发环境一般可以用cheap-module-source-map类型，便于错误调试；

  

* 生产环境可以做的优化：

  1. 代码压缩；

     * CSS：mini-css-extra-plugin提取CSS代码到单独文件，通过css-loader的minimize压缩CSS；
     * JS：terser-webpack-plugin压缩文件；
     * 图片：image-webpack-plugin压缩图片。

  2. 代码调试-source-map生产环境可以用hidden-source-map类型，隐藏源代码而且也会提示错误信息；

  3. Tree-shaking 去除无用代码，生产环境会自动开启，可以配置sideEffects来设置哪些文件是不需要tree-shaking的；

     



扩展：

1⃣️ webpack的作用

2⃣️ webpack核心打包原理

3⃣️ webpack HMR原理

4⃣️ webpack代码分割和公共代码提取

5⃣️ webpack路由懒加载原理

6⃣️ webpack5和webpack4的区别

7⃣️ webpack的loader和plugin



参考链接：

1. https://zhuanlan.zhihu.com/p/168517867
1. https://juejin.cn/post/6844904094281236487#heading-15
1. https://juejin.cn/post/6943468761575849992
1. https://juejin.cn/post/7002839760792190989#heading-2

