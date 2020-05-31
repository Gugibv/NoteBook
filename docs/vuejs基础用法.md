笔记来源：《[2019年最全最新Vue、Vuejs教程，从入门到精通](https://www.bilibili.com/video/BV15741177Eh?from=search&seid=1193450960557808037)》

### 一、邂逅Vuejs

- vue 读音 /vju:/ ，类似于 view
- vue是一个渐进式的框架，渐进式意味着你可以将vue作为你应用的一部分嵌入其中，带来更丰富的交互体验
- vue有很多特点和web开发中常见的高级功能
  - 解耦视图和数据
  - 可复用的组件
  - 前端路由技术
  - 状态管理
  - 虚拟DOM

#### 1.1、vue.js安装

安装Vue的方式有很多：

方式一：直接CDN引入

```js
<!-- 开发环境版本，包含了有帮助的命令行警告 --> 
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

方式二：下载和引入

```js
开发环境 https://vuejs.org/js/vue.js 

生产环境 https://vuejs.org/js/vue.min.js
```

方式三：NPM安装

后续通过`webpack`和`CLI`的使用，我们使用该方式

#### 1.2、小案例-计数器

```js
 <div id="app">
       <div class="input-num">
         <button @click="sub">-</button>
         <span>当前计数：{{num}}</span>
         <button @click="add">+</button>
       </div>
 </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
       var app = new Vue({
        el: '#app',
        data: {
          num: 1
        },
        methods:{
            add:function(){
                this.num++
            },
            sub:function(){{
                this.num--
            }
        }
      })
      </script>
```

#### 1.3、vue的mvvm

<div align="center"> <img src="pics/vue的mvvm.jpg" width="500"/> </div><br>View层：

- 视图层，在我们前端开发中，通常就是DOM层。主要的作用是给用户展示各种信息。

Model层：

- 数据层，数据可能是我们固定的死数据，更多的是来自我们服务器，从网络上请求下来的数据。

VueModel层：

- 视图模型层，视图模型层是View和Model沟通的桥梁。
- 一方面它实现了Data Binding，也就是数据绑定，将Model的改变实时的反应到View中
- 另一方面它实现了DOM Listener，也就是DOM监听，当DOM发生一些事件(点击、滚动、touch等)时，可以监听到，并在需要的情况下改变对应的Data。

> MVVM维基百科解释：`MVVM`有助于将图形用户界面的开发与业务逻辑或后端逻辑（数据模型）的开发分离开来，这是通过置标语言或`GUI`代码实现的。`MVVM`的视图模型是一个值转换器，这意味着视图模型负责从模型中暴露（转换）数据对象，以便轻松管理和呈现对象。在这方面，视图模型比视图做得更多，并且处理大部分视图的显示逻辑。视图模型可以实现中介者模式，组织对视图所支持的用例集的后端逻辑的访问。

计数器的MNNM，它们之间如何工作呢？

- 首先ViewModel通过Data Binding让obj中的数据实时的在DOM中显示。
- 其次ViewModel通过DOM Listener来监听DOM事件，并且通过methods中的操作，来改变obj中的数据。

有了Vue帮助我们完成VueModel层的任务，在后续的开发，我们就可以专注于数据的处理，以及DOM的编写工作了。

#### 1.4、Vue的options选项

你会发现，我们在创建Vue实例的时候，传入了一个对象options。

这个options中可以包含哪些选项呢？详细解析：[选项-数据](https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E6%95%B0%E6%8D%AE)

目前掌握这些选项：

el：决定之后Vue实例会管理哪一个DOM

data：Vue实例对应的数据对象（组件当中data必须是一个函数）

methods：定义属于Vue的一些方法，可以在其他地方调用，也可以在指令中使用

#### 1.5、Vue的生命周期