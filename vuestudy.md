# vue核心概念

## 1.速写语法糖

### 属性

<img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210517192401067.png" alt="image-20210517192401067" style="zoom:50%;" />

### 方法

<img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210517192458786.png" alt="image-20210517192458786" style="zoom: 50%;" /><img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210517192525608.png" alt="image-20210517192525608" style="zoom:50%;" />

### 模板字符串

反引号，需要拼接的用  ``  ,$ { 里面放js语法表达式 }，可直接换行

<img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210517192729031.png" alt="image-20210517192729031" style="zoom:50%;" />

## 2.注入

配置对象中的部分内容会被提取到vue实例中：

- `data`
- `methods`

该过程称之为**注入**

注入目的有两个：

- 完成数据响应式

  vue是怎么知道数据被更改了？

  `vue2.0`是通过`Object.defineProperty`方法完成了数据响应式（但是无法感知到属性的新增或者删除），`vue3.0`是通过`Class Proxy`完成的数据响应式，，实质是vue调用了这个方法

- 绑定`this`

## 3.虚拟DOM树VDOM

### 了解浏览器渲染的过程！！

$开头提供大家使用，_开头不建议使用

**为了提高渲染效率**，vue会把模板**编译**成为虚拟DOM树，然后再生成真实的DOM

<img src="http://mdrs.yuanjin.tech/img/20200908030214.png" alt="image-20200908030214647" style="zoom:50%;" />

当数据更改时，将重新编译成虚拟DOM树，然后对前后两棵树进行比对（使用diff算法），仅将差异部分反映到真实DOM，这样既可最小程度的改动真实DOM，提升页面效率

<img src="http://mdrs.yuanjin.tech/img/20200908031642.png" alt="image-20200908031642346" style="zoom:50%;" />

​	每次渲染都生成全新的虚拟节点----》所以每次的节点不能相等；但是真实的是相同的

因此，对于`vue`而言，提升效率重点着眼于两个方面：

- 减少新的虚拟DOM的生成
- 保证对比之后，只有必要的节点有变化。

`vue`提供了多种方式生成虚拟DOM树：

1. 在挂在的元素内部直接书写，此时将使用元素的**`outerHTML`**作为模板

2. 在`template`配置中书写

3. 在`render`配置中用函数直接创建虚拟节点树，此时，完全脱离模板，将省略编译步骤；调用render函数就会生成一个虚拟dom

   ![](C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210517200259926.png)

   ![image-20210517200202744](C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210517200202744.png)

   ![image-20210517200410252](C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210517200410252.png)

这些步骤从上到下，优先级逐渐提升

**注意：虚拟节点 树 必须是单根的**===》模板节点只能有一个单节点

## 4.挂载

将生成的真实DOM树，放置到某个元素位置，称之为**挂载**

挂载的方式：

1. 通过`el:"css选择器"`进行配置
2. 通过`vue实例.$mount("css选择器")`进行配置=====》唯一区别：：可以实现延迟挂载

## 5.完整流程

<img src="http://mdrs.yuanjin.tech/img/20200908051939.png" alt="image-20200908051939745" style="zoom:50%;" />

# 模板语法

## 1.内容

`vue`中的元素内容使用`mustache`模板引擎进行解析（只有内容才能用）

> mustache：https://github.com/janl/mustache.js

## 2.指令

（只有元素才能用）

指令会影响元素的渲染行为，指令始终以`v-`开头

基础指令：

- v-for：循环渲染元素
- v-html：设置元素的`innerHTML`，该指令会导致元素的模板内容失效
- v-on：注册事件
  - 该指令由于十分常用，因此提供了简写`@`
  - 事件支持一些指令修饰符，如`prevent`
  - 事件参数会自动传递
- v-bind：绑定动态属性
  - 该指令由于十分常用，因此提供了简写`:`
- v-show：控制元素可见度
- v-if、v-else-if、v-else：控制元素生成
- v-model：双向数据绑定，常用于表单元素
  - 该指令是`v-on`和`v-bind`的复合版

> 进阶指令：
>
> - v-slot
> - v-text
> - v-pre
> - v-cloak
> - v-once
> - 自定义指令

## 3.特殊属性

最重要的特殊属性：`key`

```html
<body>
    <div id="app">
      <div v-if="loginType==='mobile'">
        <label>手机号：</label>
        <input type="text" key="1" />
      </div>
      <div v-else>
        <label>邮箱：</label>
        <input type="text" key="2" />
      </div>
      <div>
        <button @click="loginType = loginType==='mobile'?'email':'mobile'">
          切换登录方式
        </button>
      </div>
    </div>
    <script src="./vue.min.js"></script>
    <script>
      // vm：vue实例
      var vm = new Vue({
        // 配置
        el: "#app", // css选择器
        data: {
          loginType: "mobile",
        },
      });
    </script>
  </body>
```

该属性可以干预`diff算法`，在同一层级，`key`值相同的节点会进行比对，`key`值不同的节点则不会

<img src="http://mdrs.yuanjin.tech/img/20200908124317.png" alt="image-20200908124317168" style="zoom:50%;" />

在循环生成的节点中，`vue`强烈建议给予每个节点唯一且稳定的`key`值

<img src="http://mdrs.yuanjin.tech/img/20200908124819.png" alt="image-20200908124819385" style="zoom:50%;" />

删除一个节点，diff算法会识别到所有节点都发生改变，真实dom都会发生改变----》消耗大

------》用key绑定唯一的值

不能直接绑定下标i，通常从后台拿的数据会有一个id----》elm.id

> 其他特殊属性：
>
> - ref
> - is
> - slot
> - slot-scope
> - scope

## 计算属性

```js
computed: {
  // 仅访问器
  prop(){
    return ...
  }
  // 访问器 + 设置器
  fullProp: {
    get(){
      return ...
    },
    set(val){
      ...
    }
  }
}
```

计算属性和方法的区别：

- 计算属性可以赋值，而方法不行
- 计算属性会进行缓存，如果依赖不变，则直接使用缓存结果，不会重新计算；而在方法里面，如果其中一个改变，其它都会进行重新渲染
- 凡是根据已有数据计算得到新数据的无参函数，都应该尽量写成计算属性，而不是方法

# 知识补充

1. ## 箭头函数

   - 任何可以书写匿名函数的位置均可以书写箭头函数
   - 箭头函数将会绑定`this`为函数书写位置的`this`值

2. ## 模块化

   - 没有模块化的世界：全局变量污染、难以管理的依赖
   - 常见的模块化标准：CommonJS、ES6 Module、AMD、CMD、UMD

   ES6 Module：

   

# 组件概念

一个完整的网页是复杂的，如果将其作为一个整体来进行开发，将会遇到下面的困难

- 代码凌乱臃肿
- 不易协作
- 难以复用

vue推荐使用一种更加精细的控制方案——组件化开发

所谓组件化，即把一个页面中区域功能细分，每一个区域成为一个组件，每个组件包含：

- 功能（JS代码）
- 内容（模板代码）
- 样式（CSS代码）

> 由于没有构建工具的支撑，CSS代码暂时无法放到组件中

# 组件开发

## 创建组件

组件是根据一个普通的配置对象创建的，所以要开发一个组件，只需要写一个配置对象即可

该配置对象和vue实例的配置是**几乎一样**的

```js
//组件配置对象
var myComp = {
  data(){//只能为函数，不能为对象，，因为要返回对象
    return {
      // ...
    }
  },
  computed:{
    //...
  },
  methods:{
    //...
  },
  template: `....`
}
```

值得注意的是，组件配置对象和vue实例有以下几点差异：

- 无`el`
- `data`必须是一个函数，该函数返回的对象作为数据
- 由于没有`el`配置，组件的虚拟DOM树必须定义在`template`或`render`中



## 注册组件

注册组件分为两种方式，一种是**全局注册**，一种是**局部注册**

### 全局注册

一旦全局注册了一个组件，整个应用中任何地方都可以使用该组件

<img src="http://mdrs.yuanjin.tech/img/2020-02-18-10-24-44.png" style="zoom:50%;" />

全局注册的方式是：

```js
// 参数1：组件名称，将来在模板中使用组件时，会使用该名称
// 参数2：组件配置对象
// 该代码运行后，即可在模板中使用组件
Vue.component('my-comp', myComp)
```

在模板中，可以使用组件了

```html
<my-comp />
<!-- 或 -->
<my-comp></my-comp>
```


> 但在一些工程化的大型项目中，很多组件都不需要全局使用。
> 比如一个登录组件，只有在登录的相关页面中使用，如果全局注册，将导致构建工具无法优化打包
> **因此，除非组件特别通用，否则不建议使用全局注册**



### 局部注册

局部注册就是哪里要用到组件，就在哪里注册

<img src="http://mdrs.yuanjin.tech/img/2020-02-18-10-28-45.png" style="zoom:50%;" />

局部注册的方式是，在要使用组件的组件或实例中加入一个配置：

```js
// 这是另一个要使用my-comp的组件
var otherComp = {
  components:{
    // 属性名为组件名称，模板中将使用该名称
    // 属性值为组件配置对象
    "my-comp": myComp
  },
  template: `
    <div>
      <!-- 该组件的其他内容 -->
      <my-comp></my-comp>
    </div>
  `;
}
```

## 应用组件

在模板中使用组件特别简单，把组件名当作HTML元素名使用即可。

但要注意以下几点：

1. **组件必须有结束**

组件可以自结束，也可以用结束标记结束，但必须要有结束

下面的组件使用是错误的：

```html
<my-comp>
```

2. **组件的命名**

无论你使用哪种方式注册组件，组件的命名需要遵循规范。

组件可以使用`kebab-case 短横线命名法`，也可以使用`PascalCase 大驼峰命名法`

下面两种命名均是可以的

```js
var otherComp = {
  components:{
    "my-comp": myComp,  // 方式1
    MyComp: myComp //方式2
  }
}
```

> 实际上，使用`小驼峰命名法 camelCase`也是可以识别的，只不过不符合官方要求的规范

使用`PascalCase`方式命名还有一个额外的好处，即可以在模板中使用两种组件名

```js
var otherComp = {
  components:{
    MyComp: myComp
  }
}
```

模板中：

```html
<!-- 可用 -->
<my-comp />
<MyComp />
```

因此，在使用组件时，为了方便，往往使用以下代码：

```js
var MyComp = {
  //组件配置
}

var OtherComp = {
  components:{
    MyComp // ES6速写属性
  }
}
```

> 注意，`PascalCase`命名不可以直接在html中使用，但可以在template配置中使用
> 想想为什么？

# 组件树

一个组件创建好后，往往会在各种地方使用它。它可能多次出现在vue实例中，也可能出现在其他组件中。

于是就形成了一个组件树

<img src="http://mdrs.yuanjin.tech/img/2020-02-18-11-00-58.png" style="zoom:50%;" />

# 向组件传递数据

大部分组件要完成自身的功能，都需要一些额外的信息

比如一个头像组件，需要告诉它头像的地址，这就需要在使用组件时向组件传递数据

传递数据的方式有很多种，最常见的一种是使用**组件属性 component props**

首先在组件中申明可以接收哪些属性:

```js
var MyComp = {
  props:["p1", "p2", "p3"],
  // 和vue实例一样，使用组件时也会创建组件的实例
  // 而组件的属性会被提取到组件实例中，因此可以在模板中使用
  template: `
    <div>
      {{p1}}, {{p2}}, {{p3}}
    </div>
  `
}
```

在使用组件时，向其传递属性：

```js
var OtherComp = {
  components: {
    MyComp
  },
  data(){
    return {
      a:1
    }
  },
  template: `
    <my-comp :p1="a" :p2="2" p3="3"/>
  `
}
```

**注意：在组件中，属性是只读的，绝不可以更改，这叫做单向数据流**

<img src="/Users/yuanjin/Desktop/单向数据流.png" style="zoom:50%;" />

# 工程结构

见代码

# vue-cli的安装

`vue-cli`是一个脚手架工具，它集成了诸多前端技术，包括但不仅限于：

- `webpack`
- `babel`
- `eslint`
- `http-proxy-middleware`
- `typescript`
- `css pre-prosessor`
- `css module`
- ....

这些工具，他们大部分都要依赖两个东西：

- node环境：很多工具的运行环境
- npm：包管理器，用于下载各种第三方库

> 目前，npm已和node集成，当安装node后，会自动安装npm

## 安装node

下载node：https://nodejs.org/zh-cn/

## 验证安装

打开终端，查看node和npm版本，验证是否安装成功:

```shell
node -v
npm -v
```

> 如果安装之前打开了终端，需要在安装后关闭终端，重新打开

## 配置源地址

默认情况下，`npm`安装包时会从国外的地址下载，速度很慢，容易导致安装失败，因此需要先配置`npm`的源地址

使用下面的命令更改`npm`的源地址为淘宝源

```shell
npm config set registry http://registry.npm.taobao.org/
```

更改好了之后，查看源地址是否正确的被更改

```shell
npm config get registry
```

## 安装vue-cli

使用下面的命令安装`vue-cli`工具

```shell
npm install -g @vue/cli
```

安装好之后，检查`vue-cli`是否安装成功

```shell
vue --version
```



# vue-cli的使用

## 在终端中进入某个目录

选择一个目录，该目录将放置你的工程文件夹

在终端中进入该目录

## 搭建工程

使用`vue-cli`提供的命令搭建工程

```shell
vue create 工程名
```

> 注意：工程名只能出现英文、数字和短横线

# 理解工程结构

视频描述

# 运行脚手架项目

下载一个项目要npm install的原因：：上传项目不上传<img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210518160411510.png" alt="image-20210518160411510" style="zoom:50%;" />

因为很大，npm就可以 把下载下来

在style标签后加scoped表示这个style里的类名都是局部的，避免全局污染 

## 导入

导入必须写绝对路径，否则回去node_modules路径里面去找

# bili-app

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

![image-20210519151530296](C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210519151530296.png)

属性：[props]

item里：

```
<div class="item" :class="{active:isActive}">
props: {

  isActive: {

   type: Boolean, //约束该属性的类型是boolean

   // required: true, //约束该属性必须要传递

   default: false,

  },

 },
```

:class="{active:isActive}" 添加类，active是否为true，传递过来----》由谁传递过来呢？？

app里：

```
 <TitleMenu :isActive="true">
```

​			如果isActive前不加冒号，获取的是字符串

避免直接对数据赋值，否则会打破单向数据流

数据是父组件的，事件是子组件的

![image-20210520205652821](C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210520205652821.png)

**必须是函数对象，这样每次调用的都是 函数，修改内容后互不影响**

### 列表组件结构图

![列表组件结构图](C:\Users\zyy\Desktop\Practical project Note\vuestudy.assets\列表组件结构图.jpg)

### 异步数据和组件的重渲染

![异步数据和组件的重渲染](C:\Users\zyy\Desktop\Practical project Note\vuestudy.assets\异步数据和组件的重渲染.jpg)

### 组件生命周期图

![组件生命周期](C:\Users\zyy\Desktop\Practical project Note\vuestudy.assets\组件生命周期.png)