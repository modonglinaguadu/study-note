# postCss-cssnext

### 自带autoprefixer

在cssnext中已经包含了autoprefixer功能，如果您使用cssnext，那就没必要安装autoprefixer插件

### 使用css未来特性

cssnext可以让我们使用CSS未来的特性，其会对这些特性做相关的兼容性处理。

------------------------------------------

#特性


下面我们将一一介绍cssnext的特性


### 自定义属性

在cssnext中，css的编写类似js，有自定义属性，这个类似sass中的$

```css

:root{
    --normal-size:12px;
}
.box{
    font-size:var(--normal-size);
}

```

输出结果是:

```css

.box{
    font-size:12px;
}
    
```

通过上面的例子，我们可以看到:

> 规则

* 我们需要把变量声明放在**:root**中
* 变量名以**--**符号开头
* 属性使用是以`var(变量名)`的形式使用


### 自定义属性集

在cssnext中，除了刚刚的自定义属性，还有自定义属性集合，类似sass中的extend，我们还是来看例子吧

```css

:root{
    --normal-color:#f66;
    --normal-size:12px;
    --normal-mixin:{
        font-size:var(--normal-size);
        color:var(--normal-color);
    }
}
.box{
    @apply --normal-mixin;
}

```

输出结果:

```css

.box{
    font-size:12px;
    color:#f66;
}

```

通过上面的例子，我们可以看到，自定义属性集就是把我们常用的，规范的，或者说组件化的(比如把画三角形的css打包成一个属性集)管理成集合

> 规则

* 和自定义属性一样，自定义属性集合写要把声明放在**:root**中
* 变量名以**--**符号开头
* 属性集的使用以`@apply 变量名`的形式使用


### calc结合自定义属性使用

css3中calc属性可以用于我们css中日常的运算，现结合自定义属性来使用。

```css

:root{
    --normal-color:#f66;
    --normal-size:12px;
    --normal-mixin:{
        font-size:calc(var(--normal-size) * 2);
        color:var(--normal-color);
    }
}
.box{
    @apply --normal-mixin;
}

```
运行结果为:

```css

.box{
    font-size:24px;
    color:#f66;
}

```

### 自定义媒体查询


自定义媒体查询我个人理解就是媒体查询范围的自定义属性，说白了就和自定义属性一样，都是用于规范化统一管理。只是自定义属性只能用于样式，而自定义媒体查询只能用于媒体查询的类型和功能


```css

@custom-media --viewport-max-s only screen and (max-width:300px);
//其实变量名后面就是一个字符串嘛~.~，既然是字符串，那就可以字符串拼接

@custom-media --viewport-min-s (min-width:300px);

@media (--viewport-max-s){
  body{
    font-size:12px;
  }
}


//变量转换之后还是字符串^-^,所以还可以拼接
@media only screen and (--viewport-min-s){
  body{
    font-size:16px;
  }
}

```

运行结果为:

```css

@media only screen and (max-width:300px){
  body{
    font-size:12px;
  }
}
@media only screen and (min-width:300px){
  body{
    font-size:16px;
  }
}

```

> 规则

* 变量名以**--**开头，因为cssnext变量名命名都是这个规则，下面我就不再说明。
* 声明：@custom-media 变量名 媒体功能
* 使用：@media (变量名)

### 媒体查询范围

就是作者觉得`max-width`和`min-width`写法很操蛋，为了舒适，简化写法,这个可以直接用于@media,也可以用于@custom-media中。

```css

@custom-media --viewport-range (width >= 300px) and (width <= 600px);

@media (--viewport-range){
  body{
    font-size:12px;
  }
}
@media (width >= 600px) and (width <= 1000px){
  body{
    font-size:14px;
  }
}

@media (1000px <= width <= 1200px){
  body{
    font-size:16px;
  }
}

```

执行结果:

```css

@media (min-width: 300px) and (max-width: 600px){
  body{
    font-size:12px;
  }
}
@media (min-width: 600px) and (max-width: 1000px){
  body{
    font-size:14px;
  }
}

@media (min-width: 1000px) and (max-width: 1200px){
  body{
    font-size:16px;
  }
}

```

> 官方规则如下

```

<mf-range> = <mf-name> [ '<' | '>' ]? '='? <mf-value>
           | <mf-value> [ '<' | '>' ]? '='? <mf-name>
           | <mf-value> '<' '='? <mf-name> '<' '='? <mf-value>
           | <mf-value> '>' '='? <mf-name> '>' '='? <mf-value>

```

### 自定义选择器

什么是自定义选择器呢，前面我们有了自定义属性，自定义属性就是为了方便样式属性管理，可是我们选择器也要管理呀，所以就有了自定义选择器


```css

@custom-selector :--wrap .box,.wrap;
@custom-selector :--any-link :link, :visited;
@custom-selector :--box .slide item;

:--wrap:--any-link{
  color:red;
}

//这个会得注意以下，别以为会转换成.xx .box,.wrap
.xx :--wrap {
  width:100px;
}

.aa,:--wrap{
  height:100px
}

.xx:--any-link{
  background-color:#f66;
}

.gg,:--box{
  font-size:14px;
}

```

运行结果:

```css

.box:link,
.wrap:link,
.box:visited,
.wrap:visited{
  color:red;
}

.xx .box,
.xx .wrap {
  width:100px;
}

.aa,
.box,
.wrap{
  height:100px
}

.xx:link,
.xx:visited{
  background-color:#f66;
}

.gg,
.slide item{
  font-size:14px;
}

```

> 这里需要注意的是继承和并列结合生成的结果



## 小结

学了这么多，我们先停下来整理以下。

上面我们主要学的是自定义**，里面包括自定义属性(集)，自定义媒体查询，自定义选择器。

我们一个一个来回想以下，不要翻到前面看哦~，尽量的回忆。

。
。
。
。
。
。
。
。
。
。
。
。
好了，你们回忆完了，我来做一个小回忆吧

我把它们相同的地方和不同的地方做比较来回忆

### 不相同

* 自定义属性(集)

    1. 声明的地方和其他两个不同，它们需要在`:root`中声明。
    2. 它们的使用不一样，自定义属性集以`@apply 变量名`的方式使用，自定义属性以`var(变量名)`的方式使用

* 自定义媒体查询

    1. 它可以在任何一个地方声明，不过声明前需要加`@custom-media` 
    2. 它们的使用不一样，自定义媒体是`(变量名)`的方式来用

* 自定义选择器

    1. 它也可以在任何地方声明，不过它声明前需要加`@custom-selector`
    2. 它的变量名命名与上面两个有一点区别，就是需要在变量名前加`:`符号

总的来说就是它们的声明和使用不一样，多用几次我们就记住了。

### 相同

* 它们的命名都有`--`符号，只是自定义选择器命名前加`:`
* 它们的声明方式都是 `变量名 变量值`
* 它们的使用都像在拼字符串，除了自定义选择器的个别结合需要小心以外，用拼字符串的思维来分析，可能有助于我们的理解

```css

:root{
    --normal-size:12x;
    --normal-com:{
        color:red;
    }
}

@custom-selector :--title h1,h2,h3,h4;
@custom-media --normal (600px <= width <= 1200px);

@media (--normal){
   :--title ,h5,h6{
       font-size:var(--normal-size);
       @apply --normal-com;
    } 
}


```

运行结果:

```css

@media (min-width: 600px) and (max-width: 1200px){
   h1,
   h2,
   h3,
   h4,
   h5,
   h6{
        font-size:12x;
        color:red;
    } 
}

```

是不是很像字符串拼接??!!!

好了，学到这里，大家就先去练习练习吧，这玩意不能靠记，得多用，等大家熟悉了，我们再接着学。

----------------------------------------

## 参考文献

[http://cssnext.io/features/](http://cssnext.io/features/)