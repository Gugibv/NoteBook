笔记来源：《[李南江亲授-jQuery+Ajax从放弃到知根知底](https://www.bilibili.com/video/BV17W41137jn?p=4)》

### 1、如何使用jQuery

下载 jquery 库：[下载地址](https://jquery.com/download/) 

引入下载的 jquery 库。

```js
<head> 
	<meta charset="UTF-8">
    <title>Title</title>
    <script src="js/jquery-3.5.1.js"></script>
</head>
```

  编写 jQuery 代码：

```js
<script>
    //1、原生 js 的固定写法
    window.onload = function (ev) {  }

    //2、jQuery 的固定写法
    $(document).ready(function () {
    })
</script>
```

### 2、jQuery 和原生 js 的区别

```js
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/jquery-3.5.1.js"></script>

    <script>
        //1、原生 js 的固定写法
        window.onload = function (ev) {
            //1.1、通过原生的js入口函数可以拿到dom元素
            var img = document.getElementsByTagName("img")[0];
            console.log(img);
            //1.2、通过原生的js入口函数可以拿到dom元素宽高
            var width = window.getComputedStyle(img).width;
            console.log(width);
        }

        //2、jQuery 的固定写法
        $(document).ready(function () {
            //2.1、通过jQuery入口函数可以拿到dom元素
            var $img = $("img")[0];
            console.log($img);
            //2.2、通过jQuery入口函数可以拿到dom元素宽高
            var $width = $img.width;
            console.log("ready",$width)
        })
    </script>
</head>
<body>
    <img src="https://img.alicdn.com/tps/i4/TB1.HpmFpP7gK0jSZFjtKc5aXXa.gif" alt="">
</body>
```

1. 入口函数的不同，js:

   ```js
   window.onload = function(){内部放js}　
   ```

   实质就是一个事件，拥有事件的三要素，事件源，事件，事件处理程序。等到所有内容，以及我们的外部图片之类的文件加载完了之后，才会去执行。只能写一个入口函数（否则会覆盖）。jQuery:

   ```js
   $(function(){})　　或者　　$(document).ready(function(){})
   ```

   是在 html所有标签都加载之后，就回去执行。可以写多个。

2. 获取元素的方式不同

