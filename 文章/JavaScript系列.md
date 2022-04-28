1. JavaScript的作用域是由词法作用域来决定的。简单来说，词法作用域就是根据代码的位置来决定。

2. 在JavaScript中，根据词法作用域的规则，内部函数总是可以访问其外部函数声明的变量，当通过调用一个外部函数返回一个内部函数以后，即使该外部函数已经执行结束了，但是内部函数引用的外部函数的变量依然保存在内存中，我们就把这些变量的集合称为闭包。

3. 作用域链和this是两套不同的系统，它们之间基本没有太多联系。

4. 执行上下文包含了变量环境、词法环境、outer和this。执行上下文主要分三种：全局执行上下文、函数执行上下文和eval执行上下文。

5. 全局执行上下文中的this是指向window对象的。

6. 在全局环境中调用一个函数，函数内部的this指向全局的window对象。

7. 通过一个对象来调用其内部的一个方法，该方法的执行上下文中的this指向对象本身。

8. 当执行new 一个对象（CreateObj）时，JavaScript引擎做了如下四件事情：
   1. 首先创建了一个空的对象tempObj.
   2. 接着调用CreateObj.call方法，并将tempObj作为call方法的参数，这样当CreateObj的执行上下文创建时，它的this就指向了tempObj对象。
   3. 然后执行CreateObj函数，此时的CreateObj函数执行上下文中的this指向了tempObj对象。
   4. 最后返回tempObj对象。
   
9. 嵌套函数中的this不会继承外层函数的this。要解决这个问题，有两种方法：一是可以定义一个变量self，在内层函数中访问，二十直接使用箭头函数（箭头函数不会创建自己的执行上下文）。

10. JavaScript中的八种数据类型：Undefined/Null/Boolean/String/Number/BigInt/Symbol/Object

11. 原始类型的数据值都是直接保存在栈中的，引用类型的值是存在堆中的。在栈空间中只是保留了对象的引用地址。

12. 栈空间不会设置太大，主要用来存放一些的原始类型的小数据。堆空间很大，能存放很多大的数据，不过缺点是分配内存和回收内存都会占用一定的时间。

13. 产生闭包的核心： 第一步需要预扫描内部函数；第二步是把内部函数引用的外部变量保存到堆中。

14. JavaScript的变量是没有数据类型的，值才有数据类型，变量可以随时持有任何数据类型。

15. 代际假说的特点：
    1. 大部分对象在内存中存在的时间很短，简单来说，就是很多对象一经分配内存，很快就变得不可访问。
    2. 不死的对象，会活的更久。
    
16. 在V8中会把堆分为新生代和老生代两个区域，新生代中存放的是生存时间短的对象，老生代中存放的生存时间久的对象。新生区域通常只支持1-8M的容量，而老生区支持的容量就大很多了。

17. 副垃圾回收器，主要负责新生代的垃圾回收。主垃圾回收器，主要负责老生代的垃圾回收。

18. 垃圾回收器的工作流程：
    1. 标记空间中活动对象和非活动对象。
    2. 回收非活动对象所占据的内存。
    3. 做内存整理。
    
19. 优化JavaScript的执行效率：
    1. 提升单次脚本的执行速度，避免JavaScript的长任务霸占主线程，这样可以使得页面快速响应交互。
    2. 避免大的内敛脚本，因为在解析HTML的过程中，解析和编译也会占用主线程。
    3. 减少JavaScript的容量，因为更小的文件会提升下载速度，并且占用更低的内存。
    
20. async是一个通过异步执行并隐士返回Promise作为结果的函数。

21. async和await执行流程：

    ```javascript
    
    async function foo() {
        console.log('foo')
    }
    async function bar() {
        console.log('bar start')
        await foo()
        console.log('bar end')
    }
    console.log('script start')
    setTimeout(function () {
        console.log('setTimeout')
    }, 0)
    bar();
    new Promise(function (resolve) {
        console.log('promise executor')
        resolve();
    }).then(function () {
        console.log('promise then')
    })
    console.log('script end')
    
    /**解析：
    1、主协程初始化异步函数foo和bar。
    2、然后继续执行，并打印script start。
    3、遇到setTimeout，添加到宏任务列表中。
    4、执行bar函数，函数内部打印 bar start。
    5、遇到 await foo(), 执行foo函数，并打印 foo。创建一个promise对象并返回给主协程。返回的promise添加到微任务列表中。
    6、继续执行 new Promise， 打印 promise exector。
    7、 遇到resolve，添加到微任务列表。主协程继续往下执行。
    8、打印 script end.
    9、当前宏任务（task）执行完毕之前，扫描一遍微任务列表，执行第一个微任务，打印 bar end。
    10、执行第二个微任务、打印 promise then.
    12、宏任务执行完毕，执行下一个宏任务，打印 setTimeout.
    **/
    ```

22. async函数的实现原理：

    该函数是通过promise+generator来实现的。generator是通过协程来控制程序的调度。在协程中执行异步任务时，先用promise封装异步任务，如果异步任务完成，会将其结果放入微任务队列中，然后通过yield让出主线程执行权，继续执行主线程的JS，主线程的js执行完毕以后，会去扫描微任务队列，如果有任务，则取出执行，这时通过迭代器的next（result）方法，并传入任务执行结果，将主线程执行权交给协程继续执行。并且result赋值给yield表达式左边变量，从而以同步的方式实现异步编程。

    所以说async函数还是通过协程+微任务+浏览器事件循环机制来实现的。
    
23. DOM是表述HTML内部数据结构，它会将Web页面和JavaScript脚本连接起来，并过滤一些不安全的内容。这就是DOM的三个作用。

24. HTML解析器并不是等整个文档都加载完成之后再解析的，而是网络进程加载了多少数据，HTML解析器便解析多少数据。

25. CSSOM有两个作用，第一个是提供给JavaScript操作样式表的能力，第二个是为布局树的合成提供基础样式信息。

26. JS继承的实现方式：

    1. 原型链继承

       ```javascript
       function People() {
         this.name  = 'zap';
         this.favorite = ['book', 'coding', 'computer']
       }
       People.prototype.sayName = function() {
         return this.name;
       }
       
       function Person() {
       }
       Person.prototype = new People();
       let p1  = new Person();
       console.log(p1.sayName());
       
       let p2 = new Person();
       // 这个时候更改p1的favorite，会对p2的favorite造成影响
       p1.favorite.push('test');
       console.log(p1.favorite);
       console.log(p2.favorite);
       // 问题：父类构造函数中的引用类型（对象、数组）,会被所有的子实例共享，数据不能进行隔离。
       ```

    2. 构造函数继承

       ```javascript
       function People() {
         this.name  = 'zap';
         this.favorite = ['book', 'coding', 'computer']
         this.showFavorite = function() {
           return this.favorite
         }
       }
       People.prototype.sayName = function() {
         return this.name;
       }
       
       function Person() {
         People.call(this);
       }
       let p1  = new Person();
       //这里会报错
       // console.log(p1.sayName());
       
       let p2 = new Person();
       // 这个时候更改p1的favorite，会对p2的favorite造成影响
       p1.favorite.push('test');
       // ['book', 'coding', 'computer', 'test']
       console.log(p1.favorite);
       // ['book', 'coding', 'computer']
       console.log(p2.favorite);
       console.log(p2.favorite);
       
       //问题:构造函数继承解决了数据共享的问题，但是会带来两个问题：一个是不能继承父类原型链上的方法，如果要继承父类的方法，都需要写在构造函数中；二是如果方法写在构造函数中，那么每次实例化一个子类，方法都要重新创建，从性能上来讲，是一种损失。
       ```

    3.  组合继承

       ```javascript
        function People() {
          this.favorite = ['book', 'coding'];
        }
       People.prototype.getFavorite = function() {
         return this.favorite;
       }
       
       function Person(name) {
         People.call(this);
         this.name = name;
       }
       Person.prototype = new People();
       // 需要重新设置子类的constructor，Person.prototype = new People()相当于子类的原型对象完全被覆盖了
       Person.prototype.constructor = Person;
       
       let p1 = new Person('zhangsan');
       let p2 = new Person('lisi');
       
       p1.favorite.push('p1-add')
       
       console.log(p1);
       console.log(p2);
       
       // 问题：父类构造函数被调用了两次
       ```

    4. 寄生组合继承

       ```javascript
        function Parent() {
               this.flag = 'parent';
               this.name = 'zhangaiping';
           }
           Parent.prototype.sayName = function() {
               return this.name;
           }
           function Child(topic) {
               Parent.call(this);
               this.topic = topic;
           }
       
           function inherit(child, parent) {
               var prototype = createObj(parent.prototype);
               prototype.constructor = child;
               child.prototype = prototype;
           }
       
           function createObj(o) {
               function F(){};
               F.prototype = o;
               return new F();
           }
           let p1 =  new Child('teacher');
           p1.name = 'zhangsan'
           let p2 = new Child('student');
           p2.name = 'lisi';
           console.log(p1);
           console.log(p2);
       ```

    5. ES6继承

       ```javascript
       class Parent {
         constructor() {
           this.name = 'fedaily'
         }
       
         getName() {
           return this.name
         }
       }
       
       class Child extends Parent {
         constructor() {
           // 这里很重要，如果在this.topic = 'fe'后面调用，会导致this为undefined，具体原因可以详细了解ES6的class相关内容，这里不展开说明
           super()
           this.topic = 'fe'
         }
       }
       
       const child = new Child()
       child.getName() // fedaily
       ```

       

27. 