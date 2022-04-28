1. script标签上defer的作用：

   1. 浏览器会异步下载该文件并不会影响后续的DOM渲染。

   2. 如果设置了有多个defer的script，则浏览器会顺序下载执行script中的内容。

   3. defer脚本会在文档渲染完毕以后，DOMContentLoaded事件调用之前执行。

      应用场景：如果脚本是依赖于页面中的dom元素（文档是否解析完毕），或者被其他脚本文件所依赖。

2. script标签上async属性的作用：

   1. async的设置，会使得script脚本异步加载并在允许的情况下执行。

   2. async的执行顺序是根据下载速度来决定的，下载完毕即可执行，如果下载速度足够快，可以在DOMContentLoaded之前执行，也可以在DOMContentLoaded之后执行）。

      应用场景：脚本不关心页面中的DOM元素，并且也不会产生其他脚本需要的数据。

3. 