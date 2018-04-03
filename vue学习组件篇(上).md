## 组件介绍:

组件系统是 Vue.js 中一个重要的概念,他提供了一种抽象,让我们可以独立可复用的小组件来构建大型应用,任意类型的应用界面都可以抽象为一个组件树.

![](http://cn.vuejs.org/images/components.png)

### 什么是组件?

**组件可以扩展 HTML 元素,封装可重用的 HTML 代码,我们可以将组件看做自定义的 HTML 元素**

### 组件的创建和注册

基本步骤:

* 创建组件构造器
* 注册组件
* 使用组件

![](http://images2015.cnblogs.com/blog/341820/201606/341820-20160629071629249-2016080485.png)

下面代码演示了三个步骤:

```html
<!-- html -->
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>JS Bin</title>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
  <div id="app">
   <my-component></my-component>
  </div>
</body>
</html>
```

```js
var myComponent = Vue.extend({
  template: "<div>miaomiao</div>"
});

Vue.compnent("my-compnent", myComponent);
var app = new Vue({
  el: "#app"
});
```

实例: http://js.jirengu.com/bekadotoje/1/edit?html,js,output

可以看到,使用组件和使用普通的 HTML 元素没什么区别.

### 理解组件的创建和注册

我们用以下步骤来理解组件的创建和注册:

1.  `Vue.extend()`是 Vue 构造器的扩展,调用`Vue.extend()`创建的是一个组件构造器.
2.  `Vue.extend()`构造器有一个选项对象,选项对象的`template`属性用于定义组件要渲染的 HTML.
3.  使用`Vue.component()`注册组件时,需要提供两个参数,第一个参数是**组件的自定义标签**,第二个是**组件构造器**.
4.  组件应该挂载到某个 Vue 实例下,否则它不会生效

请注意第 4 点,下面代码在 3 个地方使用了`<my-component>`标签,但只有#app1 和#app2 下的`<my-component>`标签才起到作用.

```html
<!DOCTYPE html>
<html>
    <body>
        <div id="app1">
            <my-component></my-component>
        </div>

        <div id="app2">
            <my-component></my-component>
        </div>

        <!--该组件不会被渲染-->
        <my-component></my-component>
    </body>
    <script src="js/vue.js"></script>
    <script>
        var myComponent = Vue.extend({
            template: '<div>This is a component!</div>'
        })

        Vue.component('my-component', myComponent)

        var app1 = new Vue({
            el: '#app1'
        });

        var app2 = new Vue({
            el: '#app2'
        })
    </script>
</html>
```

示例: http://js.jirengu.com/gevujenama/1/edit?html,js,output

![](https://i.loli.net/2018/04/03/5ac2ff55c3909.png)

### 全局注册和局部注册

调用`Vue.component`注册组件时,组件的注册是全局的,这意味着可以在任意的**Vue 实例**下使用.
如果不需要全局注册,或者是让组件使用在其它组件内,可以用选项对象的`components`属性实现局部注册.

上面的例子改成局部注册:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>JS Bin</title>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
    <div id="app1">
      <my-component></my-component>
    </div>

    <div id="app2">
      <my-component></my-component>
    </div>
</body>
</html>
```

```js
//1. 构建组件器
var myComponent = Vue.extend({
  template: "<div>miaomiao</div>"
});

//全局注册组件
// Vue.component('my-component',myComponent)

var app1 = new Vue({
  el: "#app1",
  //2. 将myComponent组件注册到Vue实例app1下
  components: {
    "my-component": myComponent
  }
});

var app2 = new Vue({
  el: "#app2"
});
```

示例: http://js.jirengu.com/sebikohaze/1/edit?html,js,output

> 由于`my-component`组件是注册在#app1 元素对应的 Vue 实例下的,所以他不能在其它 Vue 实例下使用.

如果这样做了.浏览器会提示一个错误:

![](https://i.loli.net/2018/04/03/5ac301ce06255.png)

### 父组件和子组件

我们可以在组件中定义并使用其它组件,这就构成了父子组件的关系.举例:
http://js.jirengu.com/biwefuzina/1/edit?html,output

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>JS Bin</title>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
    <div id="app">
      <parent-component></parent-component>
    </div>

  <script>
      //儿子的模板构造
    var Child = Vue.extend({
            template: '<p>This is a child component!</p>'
        })

    //父亲模板构造
    var Parent = Vue.extend({
        // 在Parent组件内使用<child-component>标签
        template :'<p>This is a Parent component<child-component></child-component></p>',
        components: {
            // 局部注册Child组件，该组件只能在Parent组件内使用
            'child-component': Child
        }
    })

    // 全局注册Parent组件
    Vue.component('parent-component', Parent)

    new Vue({
        el: '#app'
    })
  </script>
</body>
</html>
```
> 注意关于`template(模板):` 对应的html外围只能有一个标签包围,不能写成`<p>This is a Parent component</p><child-component></child-component>`,否则无效.

> 解析上面代码的构建组件过程:
1. `var Child = Vue.extend(...)`定义一了个Child组件构造器
2. `var Parent = Vue.extend(...)`定义一个Parent组件构造器
3. `template :'<p>This is a Parent component</p><child-component></child-component>'`，在Parent组件内以标签的形式使用Child组件。
4. `components: { 'child-component': Child }`，将Child组件注册到Parent组件，并将Child组件的标签设置为child-component。
5. `Vue.component('parent-component', Parent)` 全局注册Parent组件
6. 在页面中使用<parent-component>标签渲染Parent组件的内容，同时Child组件的内容也被渲染出来

**Child组件时在Parent组件中注册的,他只能在Parent组件中使用,确切来说,子组件只能在父组件的template中使用**

请注意下面两种子组件的使用方法是错误的: 
1. **以子标签的相识在父组件中使用**
```html
<div id="app">
    <parent-component>
        <child-component></child-component>
    </parent-component>
</div>
```
为什么会无效呢? 因为当子组件注册到父组件时,Vue.js会编译好父组件的模板,模板(template)的内容已经决定了父组件将要渲染的HTML.
`<parent-component>…</parent-component>`相当于运行时,它的一些子标签只会被当做普通的HTML来执行,`<child-component></child-component>`不是标准的HTML标签，会被浏览器直接忽视掉。

2.在父组件标签外使用子组件
```html
<div id="app">
     <parent-component>
    </parent-component>
    <child-component>
    </child-component>
</div>
```
运行这段代码,浏览器会提示以下错误:
![](https://images2015.cnblogs.com/blog/341820/201606/341820-20160629071647531-1630279604.png)


### 组件注册语法糖
以上组件注册的方式有些繁琐,Vue.js为了简化这个过程, 提供了注册语法糖.
**使用Vue.component()直接创建和注册组件**
```js
Vue.component('my-component',{
  template: '<div>This is the first component!</div>'
})
var app = new Vue({
  el: '#app1',
})
```
> 与之前的区别在于,省略掉了`Vue.extend()`构建组件器的过程,直接将构建过程中的template放进了注册组件`Vue.component()`中
`Vue.component()`的第一个参数是**标签名**,第二个参数是一个选项参数,使用选项参数的**template属性**定义组件模板.
使用这种方式,Vue在背后会自动地调用`Vue.extend()`

> 可以这么理解,Vue.component()中的参数,是自定义标签和标签对应的template HTML.

**在选项对象的components属性中实现局部注册**
```JS
var app = new Vue({
  el: '#app',
  components: {
    //局部注册,my-components1是标签名称
    'my-components1': {
      template: '<div>This is the second component!</div>'
    },
    'my-components2' : {
      template: '<div>This is the third component!</div>'
    }
  }
})
```


#### 使用script标签或template标签
尽管语法糖简化了组件注册,但在template选项中拼接HTML元素比较麻烦,这也导致了HTML和JavaScript的高耦合性.
庆幸的是,Vue.js提供了两种方式将定义在JavaScript中的HTML模板分离出来.

**使用`<script>`标签**
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>JS Bin</title>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    <my-component></my-component>
  </div>
  
  <script type="text/x-template" id="myComponent">
      <div>This is a component!</div>
  </script>
  
  <script>
    Vue.component('my-component',{
      template: '#myComponent'
    })
    
    new Vue({
      el: '#app'
    })
  </script>
</body>
</html>
```
示例: http://js.jirengu.com/fiserupaxa/1/edit?html,output

> 解析: template选项现在不再是HTML元素,而是一个id,Vue.js根据这个id查找对应的元素,然后将这个元素内的HTML作为模板进行编译.
注意: 使用`<script>`标签时,type指定为`text/x-template`,意在告诉浏览器这不是一段js脚本,浏览器在解析HTML文档时会忽略`<script>`标签内定义的内容

### 使用`<template>标签`
如果使用`<template>`标签,则不需要指定type属性.
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>JS Bin</title>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    <my-component></my-component>
  </div>
  
  <template id="myComponent">
       <div>This is a component!</div>
  </template>
  
  <script>
    Vue.component('my-component',{
      template: '#myComponent'
    })
    
    new Vue({
      el: '#app'
    })
  </script>
</body>
</html>
```
实例: http://js.jirengu.com/nikazabeno/1/edit?html,output

在理解组件的创建和注册过程后,建议使用`<script>`或`<template>`标签来定义组件的HTML模板.
这使得HTML代码和JavaScript是分离的,便于阅读和维护.

另外在Vue.js中,可创建.vue后缀的文件,在.vue文件中定义组件,这个内容以后探讨.


### 组件的el和data选项
传入Vue构造器的多数选项也可以用在`Vue.entend()`或`Vue.component()`中,不过有两个特例: `data`和`el`
Vue.js规定: **在定义组件的选项是,data和el选项必须使用函数**
下面的代码在执行时,浏览器会提出一个错误
```js
Vue.component('my-component',{
  data: {
    a : 1
  }
})
```
![](https://i.loli.net/2018/04/03/5ac325c9dda67.png)

另外,如果data选项指向一个对象,这意味着所有组件实例共用一个data.我们应该使用一个函数作为data选项,让这个函数返回一个新对象.

```js
Vue.component('my-component',{
  data: function(){
    return { a : 1 }
  }
})
```

### 使用props
**组件实例的作用域是孤立的**.这意味着不能并且不应该在子组件的模板内之间引用父组件的数据.可以使用**props**把数据传给子组件.

### props基础示例
下面代码定义了一个子组件my-component,在Vue实例中定义了data选项.
```js
var app = new Vue({
  el: '#app',
  data: {
    name: 'Tom',
    age: 28
  },
  components: {
    'my-component': {
      template: '#myComponent',
      props: ['myName','myAge']
    }
  }
})
```

为了便于理解,你可以将这个Vue实例看做是`my-component`的父组件.如果我们想使用父组件的数据,则必须现在子组件中定义`props`属性,也就是 `props: ['myName','myAge']`这行代码.**props对应的是一个数组.** 

定义子组件的HTML模板:
```html
<template id="myComponent">
    <table>
        <tr>
            <th colspan="2">
                子组件数据
            </th>
        </tr>
        <tr>
            <td>my name</td>
            <td>{{ myName }}</td>
        </tr>
        <tr>
            <td>my age</td>
            <td>{{ myAge }}</td>
        </tr>
    </table>
</template>
```
将父组件数据通过已定义好的props属性传递给子组件:
```html
<div id="app">
    <my-component v-bind:my-name="name" v-bind:my-age="age"></my-component>
  </div>
```
> 注意: 在子组件中定义prop时,使用了`camelCase`命令法.由于HTML特性不区分大小写,camelCase的prop用于特性时,需要转位`kebab-case`(短横线隔开).例如,在prop中定义的myName,在用作特性时需要转换为my-name.

实例: http://js.jirengu.com/gedeqeniru/1/edit?html,output

父组件时如何将数据传给子组件的呢? 相信看了下面的图,就能很好的理解了.

![](https://images2015.cnblogs.com/blog/341820/201606/341820-20160629071700437-2088181944.png)

在父组件中使用子组件时,通过以下语法将数据传递给子组件:

```html
<child-component v-bind:子组件prop="父组件数据属性"></child=component>
```
`v:bind=my-name="name"`  //my-name不要引号

### prop的绑定类型

#### 单向绑定
既然父组件将数据传递给了子组件,那么如果子组件修改了数据,对父组件是否会有所影响呢?我们将子组件模板和页面HTML稍作更改:
```html
<div id="app">
    <table>
      <tr>
        <th colspan="3">父组件数据</th>
      </tr>
      <tr>
        <td>name</td>
        <td>{{name}}</td>
        <td><input type="text" v-model="name"></td>
      </tr>
      <tr>
        <td>age</td>
        <td>{{ age }}</td>
        <td><input type="text" v-model="age"></td>
      </tr>
    </table>
    
    <my-component v-bind:my-name="name" v-bind:my-age="age"></my-component>
  </div>
  
  <template id="myComponent">
    <table>
      <tr>
        <th colspan="2">
          子组件数据
        </th>
      </tr>
      <tr>
        <td>my name</td>
        <td>{{ myName }}</td>
        <td><input type="text" v-model="myName"></td>
      </tr>
      <tr>
        <td>my age</td>
        <td>{{ myAge }}</td>
        <td><input type="text" v-model="myAge"></td>
      </tr>
    </table>
  </template>
```

```js
var app = new Vue({
      el: '#app',
      data: {
        name: 'Tom',
        age: 28
      },
      components: {
        'my-component': {
          template: '#myComponent',
          props: ['myName','myAge']
        }
      }
    })
```
实例: http://js.jirengu.com/nutohinuvo/1/edit?html,output
运行这个页面,我们做两个小试验:
1. 在页面修改子组件的数据
![](https://images2015.cnblogs.com/blog/341820/201606/341820-20160629071703452-1344564.gif)
**修改了子组件的数据,没有影响父组件的数据.**

2. 在页面上修改父组件的数据
![](https://images2015.cnblogs.com/blog/341820/201606/341820-20160629071706734-475641769.gif)
**修改了父组件的数据,并且影响了子组件**

> prop默认是**单向绑定**: 当父组件的属性变化时,将传到给子组件,但是反过来不会.这是为了防止子组件无意修改了父组件的状态.

### 双向绑定
可以使用`.sync`显式地指定双向绑定,这使得子组件的数据修改会回传给父组件
但是vue2.0被弃用以后更新.
文档: https://vuefe.cn/v2/guide/components.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E4%BA%8B%E4%BB%B6

对于子组件，如果想要更新 foo 的值，则需要显式地触发一个事件，而不是直接修改 prop：

`this.$emit('update:foo', newValue)`

### 单次绑定(同样废除)
以后更新...