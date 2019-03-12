chapter17-JQuery



# 一 jQuery是什么？  

［1］   jQuery由美国人John Resig创建，至今已吸引了来自世界各地的众多 javascript高手加入其team。

［2］   jQuery是继prototype之后又一个优秀的Javascript框架。其宗旨是——WRITE LESS,DO MORE!

［3］  它是轻量级的js库(压缩后只有21k) ，这是其它的js库所不及的，它兼容CSS3，还兼容各种浏览器

［4］  jQuery是一个快速的，简洁的javaScript库，使用户能更方便地处理HTMLdocuments、events、实现动画效果，并且方便地为网站提供AJAX交互。

［5］  jQuery还有一个比较大的优势是，它的文档说明很全，而且各种应用也说得很详细，同时还有许多成熟的插件可供选择。



# 二 什么是jQuery对象？

jQuery 对象就是通过jQuery包装DOM对象后产生的对象。jQuery 对象是 jQuery 独有的**.** 如果一个对象是 jQuery 对象**,** 那么它就可以使用 jQuery 里的方法: $(“#test”).html();

JQuery库中的所有封装方法，都基于jquery对象调用。

jquery对象：两种方式完全一样

Jquery.方法

$.方法

```js
$("#test").html() 
   
意思是指：获取ID为test的元素内的html代码。其中html()是jQuery里的方法 

这段代码等同于用DOM实现代码： document.getElementById(" test ").innerHTML; 

虽然jQuery对象是包装DOM对象后产生的，但是jQuery无法使用DOM对象的任何方法，同理DOM对象也不能使用jQuery里的方法.乱使用会报错

约定：如果获取的是 jQuery 对象, 那么要在变量前面加上$. 

var $variable = jQuery 对象
var variable = DOM 对象

$variable[0]：jquery对象转为dom对象      $("#msg").html(); $("#msg")[0].innerHTML
```

 jquery的基础语法：

$(selector).action()     

selector：选择器--找标签

action()  ：对标签的操作

CSS, JS, JQuery思路一致：先找后做。

 参考：http://jquery.cuishifeng.cn/

下载：

min：Production version。压缩版。用于实际的网站中，已被精简和压缩。节省带宽，提升生产性能。

非min： Development version。没有压缩，是可读的代码。用于开发调试

如果不希望下载并存放 jQuery，那么也可以通过 CDN（内容分发网络） 引用它。谷歌和微软的服务器都存有 jQuery 。

```js
// 官网jquery压缩版引用地址:
<script src="https://code.jquery.com/jquery-3.3.1.js"></script>
```

提示：使用谷歌或微软的 jQuery，有一个很大的优势：

许多用户在访问其他站点时，已经从谷歌或微软加载过 jQuery。所有结果是，当他们访问您的站点时，会从缓存中加载 jQuery，这样可以减少加载时间。同时，大多数 CDN 都可以确保当用户向其请求文件时，会从离用户最近的服务器上返回响应，这样也可以提高加载速度。

# 三 寻找元素(选择器和筛选器) 

## 3.1   选择器

基本格式：$("")

#### 3.1.1 基本选择器      

```js
$("*")  $("#id")   $(".class")  $("element")  $(".class,p,div")
//通配选择器	#id选择器	.class选择器	标签选择器	多元素选择器(符合任意一个条件)
```

#### 3.1.2 层级选择器   

```js
$(".outer div")  $(".outer>div")   $(".outer+div")  $(".outer~div")
//后代元素选择器(所有后代)  子元素选择器(所有子元素)  毗邻元素选择器(紧随之后的一个同级元素) 普通兄弟选择器
```

#### 3.1.3 基本筛选器　　

```js
$("li:first")  $("li:eq(2)")  $("li:even") $("li:gt(1)")
```

#### 3.1.4 属性选择器

更多用在自定义属性

```js
$("[att]")  
$("[att='val']")	所有att属性为val的元素 
$("[alex='sb']")  
$("[alex='sb'][id]")	列表中指定索引位置的元素
```

#### 3.1.5 表单选择器      

在属性选择器上对input标签做了简化

```js
$("[type='text']") 等价于----->$(":text")         注意只适用于input标签: $("input:checked")
```

## 3.1.6 表单属性选择器

```js
    :enabled
    :disabled
    :checked
    :selected
    场景：首页时，上一页不能点击。同理尾页。
```

```html
<body>

<form>
    <input type="checkbox" value="123" checked>
    <input type="checkbox" value="456" checked>


  <select>
      <option value="1">Flowers</option>
      <option value="2" selected="selected">Gardens</option>
      <option value="3" selected="selected">Trees</option>
      <option value="3" selected="selected">Trees</option>
  </select>
</form>


<script src="jquery.min.js"></script>
<script>
    // console.log($("input:checked").length);     // 2

    // console.log($("option:selected").length);   // 只能默认选中一个,所以只能lenth:1

    $("input:checked").each(function(){

        console.log($(this).val())
    })

</script>


</body>
```



## 3.2 筛选器

选择器：通过固定的方法把标签都找到

筛选器：在已选择的标签里进行过滤

#### 3.2.1  过滤筛选器    

```js
$("li").eq(2)  索引
$("li").first()  
$("ul li").hasclass("test")
```

#### 3.2.2  查找筛选器（导航查找）重点　　

```js
查找子标签：         
$("div").children(".test")  //查找到的子标签中可以通过条件筛选    
查找后代标签：
$("div").find(".test")  
                               
向下查找兄弟标签：    
$(".test").next()               
$(".test").nextAll()                        
$(".test").nextUntil() 
                           
向上查找兄弟标签：    
$("div").prev()                 
$("div").prevAll()       
$("div").prevUntil() 

查找所有兄弟标签：    
$("div").siblings()  
             
查找父标签：         
$(".test").parent()   // 常用           
$(".test").parents()  // 所有父标签，直到body
$(".test").parentUntil("outer") //一直找到outer这一层，不包括outer 

// 对比：CSS导航查找
parentElement           // 父节点标签元素
children                // 所有子标签
firstElementChild       // 第一个子标签元素
lastElementChild        // 最后一个子标签元素
nextElementtSibling     // 下一个兄弟标签元素
previousElementSibling  // 上一个兄弟标签元素

// js和jQuery选择器的区别：
$(".p1").css("color","red");
jQuery选择器得到的是一个集合对象，后面的操作会循环加载
遍历集合中的所有标签，对每个标签进行同样的操作。
如何做不同的操作，自己加循环遍历。
```

```js

```



# 四 操作元素(属性，css，文档处理)

## 4.1 事件

### 页面载入

```js
ready(fn)  // 当DOM载入就绪可以查询及操纵时绑定一个要执行的函数。
$(document).ready(function(){}) -----------> $(function(){})
```

### 事件绑定

```js
//语法:  标签对象.事件(函数)    
eg: $("p").click(function(){})
```

### **事件委派：**

```js
$("").on(eve,[selector],[data],fn)  // 在选择元素上绑定一个或多个事件的事件处理函数。
$("父级标签").on("事件",[实际触发事件的标签],[data],function)
```



```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
<hr>
<button id="add_li">Add_li</button>
<button id="off">off</button>

<script src="jquery.min.js"></script>
<script>
    $("ul li").click(function(){
        alert(123)
    });

    $("#add_li").click(function(){
        var $ele=$("<li>");
        $ele.text(Math.round(Math.random()*10));
        $("ul").append($ele)

    });


//    $("ul").on("click","li",function(){
//        alert(456)
//    })

     $("#off").click(function(){
         $("ul li").off()
     })
    
</script>
```



### 事件切换

hover事件：

一个模仿悬停事件（鼠标移动到一个对象上面及移出这个对象）的方法。这是一个自定义的方法，它为频繁使用的任务提供了一种“保持在其中”的状态。

over:鼠标移到元素上要触发的函数

out:鼠标移出元素要触发的函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .test{

            width: 200px;
            height: 200px;
            background-color: wheat;

        }
    </style>
</head>
<body>


<div class="test"></div>
</body>
<script src="jquery.min.js"></script>
<script>
//    function enter(){
//        console.log("enter")
//    }
//    function out(){
//        console.log("out")
//    }
// $(".test").hover(enter,out)


$(".test").mouseenter(function(){
        console.log("enter")
});

$(".test").mouseleave(function(){
        console.log("leave")
    });

</script>
</html>
```



## 4.2 属性操作

```js
--------------------------CSS类
$("").addClass(class名|fn)
$("").removeClass([class名|fn])

--------------------------属性
$("").attr("属性名","属性值");
$("").removeAttr("属性名","属性值");
$("").prop("属性名","属性值");
$("").removeProp("属性名","属性值");

--------------------------HTML代码/文本/值
$("").html([val|fn])
$("").text([val|fn])
$("").val([val|fn|arr])
注意：
标签内如果有html标签:
.html()	取所有内容（文本和标签）
.text()	只取文本
标签内如果是纯文本，二者取出的内容一样，
---------------------------
$("#c1").css({"color":"red","fontSize":"35px"})
```



attr方法使用：

```html
<input id="chk1" type="checkbox" />是否可见
<input id="chk2" type="checkbox" checked="checked" />是否可见

<script>

//对于HTML元素本身就带有的固有属性，在处理时，使用prop方法。
//对于HTML元素我们自己自定义的DOM属性，在处理时，使用attr方法。
//像checkbox，radio和select这样的元素，选中属性对应“checked”和“selected”，这些也属于固有属性，因此
//需要使用prop方法去操作才能获得正确的结果。


//    $("#chk1").attr("checked")
//    undefined
//    $("#chk1").prop("checked")
//    false

//  ---------手动选中的时候attr()获得到没有意义的undefined-----------
//    $("#chk1").attr("checked")
//    undefined
//    $("#chk1").prop("checked")
//    true

    console.log($("#chk1").prop("checked"));//false
    console.log($("#chk2").prop("checked"));//true
    console.log($("#chk1").attr("checked"));//undefined
    console.log($("#chk2").attr("checked"));//checked
</script>
```



## 4.3 each循环

我们知道，

```js
$("p").css("color","red")　　
```

是将css操作加到所有的标签上，内部维持一个循环；但如果对于选中标签进行不同处理，这时就需要对所有标签数组进行循环遍历啦

jquery支持两种循环方式：

### 方式一

**格式：$.each(obj,fn)**

```js
li=[10,20,30,40];
dic={name:"yuan",sex:"male"};
$.each(li,function(i,x){
    console.log(i,x)
});
```

### 方式二

**格式：$("").each(fn)**

```js
$("tr").each(function(){
    console.log($(this).html())
})
```

其中,$(this)代指当前循环标签。

### each扩展

```js
/*
        function f(){

        for(var i=0;i<4;i++){

            if (i==2){
                return
            }
            console.log(i)
        }

    }
    f();  // 这个例子大家应该不会有问题吧!!!
//-----------------------------------------------------------------------


    li=[11,22,33,44];
    $.each(li,function(i,v){

        if (v==33){
                return ;   //  ===试一试 return false会怎样?
            }
            console.log(v)
    });

//------------------------------------------


    // 大家再考虑: function里的return只是结束了当前的函数,并不会影响后面函数的执行

    //本来这样没问题,但因为我们的需求里有很多这样的情况:我们不管循环到第几个函数时,一旦return了,
    //希望后面的函数也不再执行了!基于此,jquery在$.each里又加了一步:
         for(var i in obj){

             ret=func(i,obj[i]) ;
             if(ret==false){
                 return ;
             }

         }
    // 这样就很灵活了:
    // <1>如果你想return后下面循环函数继续执行,那么就直接写return或return true
    // <2>如果你不想return后下面循环函数继续执行,那么就直接写return false


// ---------------------------------------------------------------------
```



## 4.4 文档节点处理

```js
//创建一个标签对象
    $("<p>")


//内部插入

    $("").append(content|fn)      ----->$("p").append("<b>Hello</b>");
    $("").appendTo(content)       ----->$("p").appendTo("div");
    $("").prepend(content|fn)     ----->$("p").prepend("<b>Hello</b>");
    $("").prependTo(content)      ----->$("p").prependTo("#foo");

//外部插入

    $("").after(content|fn)       ----->$("p").after("<b>Hello</b>");
    $("").before(content|fn)      ----->$("p").before("<b>Hello</b>");
    $("").insertAfter(content)    ----->$("p").insertAfter("#foo");
    $("").insertBefore(content)   ----->$("p").insertBefore("#foo");

//替换
    $("").replaceWith(content|fn) ----->$("p").replaceWith("<b>Paragraph. </b>");

//删除

    $("").empty()
    $("").remove([expr])

//复制

    $("").clone([Even[,deepEven]])
```



## 4.5 动画效果

### 显示隐藏

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="jquery-2.1.4.min.js"></script>
    <script>

$(document).ready(function() {
    $("#hide").click(function () {
        $("p").hide(1000);
    });
    $("#show").click(function () {
        $("p").show(1000);
    });

//用于切换被选元素的 hide() 与 show() 方法。
    $("#toggle").click(function () {
        $("p").toggle();
    });
})

    </script>
    <link type="text/css" rel="stylesheet" href="style.css">
</head>
<body>


    <p>hello</p>
    <button id="hide">隐藏</button>
    <button id="show">显示</button>
    <button id="toggle">切换</button>

</body>
</html>
```



### 滑动

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="jquery-2.1.4.min.js"></script>
    <script>
    $(document).ready(function(){
     $("#slideDown").click(function(){
         $("#content").slideDown(1000);
     });
      $("#slideUp").click(function(){
         $("#content").slideUp(1000);
     });
      $("#slideToggle").click(function(){
         $("#content").slideToggle(1000);
     })
  });
    </script>
    <style>

        #content{
            text-align: center;
            background-color: lightblue;
            border:solid 1px red;
            display: none;
            padding: 50px;
        }
    </style>
</head>
<body>

    <div id="slideDown">出现</div>
    <div id="slideUp">隐藏</div>
    <div id="slideToggle">toggle</div>

    <div id="content">helloworld</div>

</body>
</html>
```



### 淡入淡出

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="jquery-2.1.4.min.js"></script>
    <script>
    $(document).ready(function(){
   $("#in").click(function(){
       $("#id1").fadeIn(1000);


   });
    $("#out").click(function(){
       $("#id1").fadeOut(1000);

   });
    $("#toggle").click(function(){
       $("#id1").fadeToggle(1000);


   });
    $("#fadeto").click(function(){
       $("#id1").fadeTo(1000,0.4);

   });
});



    </script>

</head>
<body>
      <button id="in">fadein</button>
      <button id="out">fadeout</button>
      <button id="toggle">fadetoggle</button>
      <button id="fadeto">fadeto</button>

      <div id="id1" style="display:none; width: 80px;height: 80px;background-color: blueviolet"></div>

</body>
</html>
```



### 回调函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="jquery-2.1.4.min.js"></script>

</head>
<body>
  <button>hide</button>
  <p>helloworld helloworld helloworld</p>



 <script>
   $("button").click(function(){
       $("p").hide(1000,function(){
           alert($(this).html())
       })

   })
    </script>
</body>
</html>
```



## 4.6 css操作

### css位置操作

```html
        $("").offset([coordinates])
        $("").position()
        $("").scrollTop([val])
        $("").scrollLeft([val])
```

示例1：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .test1{
            width: 200px;
            height: 200px;
            background-color: wheat;
        }
    </style>
</head>
<body>


<h1>this is offset</h1>
<div class="test1"></div>
<p></p>
<button>change</button>
</body>
<script src="jquery-3.1.1.js"></script>
<script>
    var $offset=$(".test1").offset();
    var lefts=$offset.left;
    var tops=$offset.top;

    $("p").text("Top:"+tops+" Left:"+lefts);
    $("button").click(function(){

        $(".test1").offset({left:200,top:400})
    })
</script>
</html>
```



示例2:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        *{
            margin: 0;
        }
        .box1{
            width: 200px;
            height: 200px;
            background-color: rebeccapurple;
        }
        .box2{
            width: 200px;
            height: 200px;
            background-color: darkcyan;
        }
        .parent_box{
             position: relative;
        }
    </style>
</head>
<body>




<div class="box1"></div>
<div class="parent_box">
    <div class="box2"></div>
</div>
<p></p>


<script src="jquery-3.1.1.js"></script>
<script>
    var $position=$(".box2").position();
    var $left=$position.left;
    var $top=$position.top;

    $("p").text("TOP:"+$top+"LEFT"+$left)
</script>
</body>
</html>
```



示例3:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style>
        body{
            margin: 0;
        }
        .returnTop{
            height: 60px;
            width: 100px;
            background-color: peru;
            position: fixed;
            right: 0;
            bottom: 0;
            color: white;
            line-height: 60px;
            text-align: center;
        }
        .div1{
            background-color: wheat;
            font-size: 5px;
            overflow: auto;
            width: 500px;
            height: 200px;
        }
        .div2{
            background-color: darkgrey;
            height: 2400px;
        }


        .hide{
            display: none;
        }
    </style>
</head>
<body>
     <div class="div1 div">
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
           <h1>hello</h1>
     </div>
     <div class="div2 div"></div>
     <div class="returnTop hide">返回顶部</div>

 <script src="jquery-3.1.1.js"></script>
    <script>
         $(window).scroll(function(){
             var current=$(window).scrollTop();
              console.log(current);
              if (current>100){

                  $(".returnTop").removeClass("hide")
              }
              else {
              $(".returnTop").addClass("hide")
          }
         });


            $(".returnTop").click(function(){
                $(window).scrollTop(0)
            });


    </script>
</body>
</html>
```



### 尺寸操作

```js
        $("").height([val|fn])
        $("").width([val|fn])
        $("").innerHeight()
        $("").innerWidth()
        $("").outerHeight([soptions])
        $("").outerWidth([options])
```

示例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        *{
            margin: 0;
        }
        .box1{
            width: 200px;
            height: 200px;
            background-color: wheat;
            padding: 50px;
            border: 50px solid rebeccapurple;
            margin: 50px;
        }

    </style>
</head>
<body>




<div class="box1">
    DIVDIDVIDIV
</div>


<p></p>

<script src="jquery-3.1.1.js"></script>
<script>
    var $height=$(".box1").height();
    var $innerHeight=$(".box1").innerHeight();
    var $outerHeight=$(".box1").outerHeight();
    var $margin=$(".box1").outerHeight(true);

    $("p").text($height+"---"+$innerHeight+"-----"+$outerHeight+"-------"+$margin)
</script>
</body>
</html>
```



# 扩展方法 (插件机制)

## jQuery.extend(object)

扩展jQuery对象本身。

用来在jQuery命名空间上增加新函数。 

在jQuery命名空间上增加两个函数:

```html
<script>
    jQuery.extend({
      min: function(a, b) { return a < b ? a : b; },
      max: function(a, b) { return a > b ? a : b; }
});


    jQuery.min(2,3); // => 2
    jQuery.max(4,5); // => 5
</script>
```



## jQuery.fn.extend(object)

扩展 jQuery 元素集来提供新的方法（通常用来制作插件）

增加两个插件方法：

```html
<body>

<input type="checkbox">
<input type="checkbox">
<input type="checkbox">

<script src="jquery.min.js"></script>
<script>
    jQuery.fn.extend({
      check: function() {
         $(this).attr("checked",true);
      },
      uncheck: function() {
         $(this).attr("checked",false);
      }
    });


    $(":checkbox:gt(0)").check()
</script>

</body>
```



