##vue 学习之旅

### Vue.js 是什么?
是一套基于构建用户界面的渐进式框架,其核心思想之一是不手动操作DOM.它通过一些特殊的HTML语法,将DOM和数据绑定起来.一旦创建了绑定,DOM将和数据保持同步,每当变更数据,DOM也将更新.

MVVM模式(Model-View-ViewModel),描述了在Vue.js中ViewModel是如何和vie和model进行交互的. plain javascript object(纯javascript对象)

![](http://cn.vuejs.org/images/mvvm.png)

ViewModel是Vue.js的核心,它是一个Vue的实例.Vue实例是作用域某个Html元素上的,这个可以是Html的body元素,也可以是指定了id的某个元素.当创建了ViewModel后,双向绑定如何达成?

首先,我们将上图的DOM Listeners和Data Bindings看做两个工具,它们是实现双向绑定的关键.

从View侧看,ViewModel中的DOM Listeners工具会帮我们监测页面上的DOM元素的变化,如果有变化,则更改Model中的数据.
从Model侧看,当我们更新Model中的数据是,Data Bindings工具会帮我们更新页面中的DOM元素.


Vue有development(开发模式),production(生产模式),none三种模式.开发模式包含完整的警告和调试模式,生产模式删除了警告.默认是生产模式.

### 初始化项目
1. 创建项目vue-demo `npm init -y`   初始化
2. 安装webpack `npm install --save webpack`(方便别人clone项目,`npm i`可以安装需要的依赖所以`--save`安装)
3. 安装Vue  `npm i --save vue`
4. 新建`webpack.config.js`配置文件(用于压缩配置:输出路径等)
5. 新建`.gitignore`文件,为后期上传git准备,以防止把这个目录上传到github上.
6. 在package.json里链接仓库地址(可选,方便他人参与项目)
```
  "repository": {
    "type": "git",
    "url": "https://github.com/FLYSASA/Hello-Vue.git"
  }
```
7. 链接github仓库,首先在远程创建好仓库. 
`git init`
`git remote add origin git@github.com:FLYSASA/Hello-Vue.git`

8. 配置webpack.config.js (输出路径,vue配置(mode模式)等,其实也是一个module.exports模块)

### 开始使用Vue
使用Vue的过程其实就是定义MVVM各个组成部分的过程.
- 定义view  即html dom
- 定义Model 即数据data
- 创建一个Vue实例或"ViewModel" 它用于连接View和Model

> 在创建Vue实例时,需要传入一个**选项对象**,选项对象包含:挂载元素(el),数据(data),方法,模生命周期函数等等.**data属性**指向Model,`data: modeldata`表示Model是`modeldata`对象(数据对象).(即data:{}  modeldata={...})
> vue.js有多种数据绑定的语法,最基础的是文本插值,使用{{}},在运行时{{message}}会被**数据对象**的**message**属性替换.

1. 新建app.js(即vue.js),引入vue
```js
//app.js
import Vue from 'vue'

var app = new Vue({   //简历view-model
  el: '#app',
  data: {
    message: ''
  }
})
```
使用Vue就是在app.js入口文件里面,引入Vue 使用import,然后创建Vue的实例,传入参数.参数是一个对象,对象有很多属性.包括el,data,methods,created等

2. 双向绑定示例: 
MVVM模式本身是实现了双向绑定的,在Vue.js中可以使用`v-model`指令在**表单**上创建双向数据绑定.
示例:http://js.jirengu.com/sosodacihe/1/edit?html,js,output

要实现双向绑定需要`v-model`属性.data对应的model对象的message属性影响着DOM上的显示,同时View上的改变也影响着message的属性值.


### Vue.js的常用指令

上面用到的`v-model`是Vue.js的常用的一个指令,那么什么是指令?

> Vue.js的指令是以`v-`开头的,它们作用与HTML元素,指令提供了一些特殊的特性,将指令绑定在元素上,指令会为绑定的目标元素添加一些特殊的行为,我们可以将指令看做是特殊的HTML特性.

Vue.js提供了一些常用的内置指令:
- v-if
- v-show
- v-else
- v-for
- v-bind
- v-on
当然我们也可以自定义指令.

### v-if指令
`v-if`是条件渲染指令,他根据表达式的真假来删除和插入元素,基本语法:
`v-if="expression"`
expression是返回bool值的表达式,表达式可以是一个bool属性(如yes no),也可以是一个返回bool属性的运算式.例如:
实例:http://js.jirengu.com/gikifaduha/1/edit?html,js,output
> 注意: `v-if`指令中赋值的变量全都要来源于Vue实例选项对象的data属性.即在data中有同名变量有对应的bool值.如`yes: true`(true不要引号)

看一个字符串中是否有另一个字符串:
`str1.indexOf('str2') >= 0`  //str1中str2的个数是否大于1,index从0开始计数.

> `v-if`指令是根据条件表达式的值来执行**元素的插入或者删除行为**

这一点可以从渲染的HTML源代码看出来,`v-if`值为false的`<h1>`元素没有渲染到HTML.

我们在验证一下,在控制台输入`app.age = 24`,会发现`老了`消失了.

> 注意: age是定义在**选项对象**的data属性中的,为什么Vue实例可以直接访问它?  这是因为**每个Vue实例都会代理其选项对象里的data属性.**

### v-show指令
`v-show`也是条件渲染指令,和`V-if`指令不同的是,使用`v-show`指令的元素始终会被渲染到HTML,它只是简单地为元素设置css的style属性.

实例: http://js.jirengu.com/xidebocigi/1/edit?html,js,output

![](https://i.loli.net/2018/04/02/5ac22ea908794.png)
> 可以看到元素不显示的原因是因为被设置了`style="display:none"`.


### v-else指令
可以用`v-else`指令为`v-if`或`v-show`添加一个`else块`.`v-else`元素必须立即跟在`v-if`或`v-show`元素的后面,否则它不能被识别.

实例:http://js.jirengu.com/leturutiqa/1/edit?html,js,output

> 解析: 
- 当`v-if`为false时,紧随后面的`v-else`才会渲染并显示.而当`v-if`为true时,紧随后面的`v-else`并不会渲染显示.
- 当`v-show`为false时,紧随后面的`v-else`也不会显示.而当`v-show`为true时,紧随后面的`v-else`也不会渲染显示.

### v-for指令
`v-for`指令基于一个数组渲染一个列表,它和javascript的遍历语法相似:
`v-for="item in items"`
items是一个数组,item是当前被遍历的数组元素.

实例:http://js.jirengu.com/huquhajaca/1/edit?html,js,output

> 解析:td代表一列,当data的属性为数组时,在DOM里需要使用`v-for="item in items"`指令,`items`与data的属性同名.
我们在选项对象的data属性中定义了一个people数组,然后在#app元素内使用`v-for`遍历people数组,将其中的数据部署到DOM上.


### v-bind指令
`v-bind`指令可以在其名称后面带一个参数,中间放一个冒号隔开,这个参数通常是HTML元素的特有属性,例如: `v-bind:class`
语法:
`v-bind:argument="expression"`  //v-bind:后面不要加空格!否则无效
下面构建了一个简单的分页条,`v-bind`指令作用于元素的class特性上.这个指令包含一个表达式,表达式含义是:高亮当前页.

> 注意:
详解href="#"与href="javascript:void(0)"的区别
"＃"包含了一个位置信息:
默认的锚点是＃top 也就是网页的上端
而javascript:void(0) 仅仅表示一个死链接
这就是为什么有的时候页面很长浏览链接明明是＃可是跳动到了页首
而javascript:void(0) 则不是如此
所以调用脚本的时候最好用void(0)

实例:http://js.jirengu.com/xuwocipixi/2/edit

> 解析: `v-bind:class='expression'`指令实则给元素又添加了一个class属性,'expression'条件满足时触发一个active属性(这里是高亮),这里activeNumber即自定义高亮号码,当遍历的li中的n等于这个值就会赋予active属性.这里是个三元运算符
> 注意: `v-for="n in pageCount"`这行代码,pageCount是整数(总页面数),遍历时n从1开始,然后遍历到pageCount结束.

### v-on指令
`v-on`指令用于监听DOM事件,语法和`v-bind`类似,后面都是冒号,例如`<a>`元素的点击事件"
```html
<a v-on:click="doSomething">
```
有两种调用形式:**绑定一个方法(让事件指向方法的引用),或者使用内联语句.**

实例:http://js.jirengu.com/devogosuju/1/edit?html,js

> 解析:`v-on:`中的方法,在js中写在methods属性中,其对应的是一个对象,里面是方法.


### `v-bind`和`v-on`的缩写
Vue.js为最常用的两个指令`v-bind`和`v-on`提供了缩写形式. **v-bind可以缩写为一个冒号":",v-on指令可以缩写为@符号.**如下:
```js
<!--完整语法-->
<a href="javascripit:void(0)" v-bind:class="activeNumber === n + 1 ? 'active' : ''">{{ n + 1 }}</a>

<!--缩写语法-->
<a href="javascripit:void(0)" :class="activeNumber === n + 1 ? 'active' : ''">{{ n + 1 }}</a>

<!--完整语法-->
<button v-on:click="greet">Greet</button>
<!--缩写语法-->
<button @click="greet">Greet</button>
```

### 综合示例
结合以上知识我们做个小Demo.

Demo:http://js.jirengu.com/jawijamepi/1/edit?html,js,output