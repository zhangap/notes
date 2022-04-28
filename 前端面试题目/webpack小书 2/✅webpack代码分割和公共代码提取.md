webpack中常见的三种代码分割方式：

1. 入口起点：使用entry配置手动分离代码；
2. 动态导入：通过动态导入模块（如import）分离代码，路由/组件懒加载用的就是这个方式；
3. 配置去重和分离规则：在webpack.config.js文件中，配置optimization.splitChunks，实现公共代码提取和代码分割。

```javascript
optimization: {
        splitChunks: {
            // minSize设置的是生成文件的最小大小，单位是字节。如果一个模块符合之前所说的拆分规则，
            // 但是如果提取出来最后生成文件大小比minSize要小，那它仍然不会被提取出来。
            minSize: 30,
            cacheGroups: {
                default: {
                    // 提取出来的公共模块将会以这个来命名，可以不配置，如果不配置，就会生成默认的文件名
                    name: 'common',
                    // all 代表所有模块，async代表只管异步加载的, initial代表初始化时就能获取的模块
                    chunks: 'initial',
                    // 当模块被不同entry引用的次数大于等于这个配置值时，才会被抽离出去
                    minChunks: 2,
                    // priority属性的值为数字，可以为负数。作用是当缓存组中设置多个拆分规则，某个模块同时符合好几个规则的时候，
                    // 则需要通过优先级属性priority来决定使用哪个拆分规则。优先级高者执行
                    priority: 1
                },
                //拆分第三方库（通过npm|yarn安装的库）
                vendors: { 
                    test: /[\\/]node_modules[\\/]/,
                    name: 'vendor',
                    chunks: 'initial',
                    priority: 3
                },
                //拆分指定文件
                c: {
                    // test的值可以是一个正则表达式，也可以是一个函数.
                    // 它可以匹配模块的绝对资源路径或chunk名称，匹配chunk名称时，将选择chunk中的所有模块。
                    test: /(src\/c\.js)$/,
                    name: 'c',
                    chunks: 'initial',
                    priority: 2
                }
            }
        }
    }
```

合理的根据需求配置optimization.splitChunks.cacheGroups，是代码分割的核心。



代码分割的本质：

在源代码直接上线和打包成唯一脚本中取一个平衡，让用户体验更好服务端也舒服。



参考链接：

1. https://juejin.cn/post/6844904001792655373
2. https://juejin.cn/post/6844904103848443912