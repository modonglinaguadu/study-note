# postCss-cssnext

# 特性

下面我们接着学习其他特性

### 选择器嵌套

这个嵌套选择器和sass的嵌套结构有点不一样，你不能用那个思维来思考，不然你可能会有点难受，你需要用数学的思维来想，好了，下面我会一一介绍各种语法和结构。

> 这个嵌套看起来很简单，但里面的逻辑其实是需要我们小心的，不然一不小心就犯错了。

#### 简单选择器嵌套

```css

.a{
  font-size:12px;
  & .b{
    font-size:12px;
  }
  @nest & .c{
    font-size:12px;
  }
  @nest .h &{
    font-size:12px;
  }
  & >.i{
    font-size:12px;
  }
  color:red;
}

```

运行结果：

```css

.a{
  font-size:12px;
}

.a .b{
  font-size:12px;
}

.a .c{
  font-size:12px;
}

.h .a{
  font-size:12px;
}

.a >.i{
  font-size:12px;
}

.a{
  color:red;
}

```

* 从上面我们可以看出，每次嵌套都需要有`&`符号，没有这个是没办法编译的。
* 里面的`&`符号，你可以把它当成上级选择器,然后两个选择器拼接起来。

```css

.a{
    color:green;
    & .b{
        color:red;
    }
}

/*结果是*/

.a{
    color:green;
}
.a .b{
    color:red;
}

```

* 在上面我们看到了`@nest`符号，这个符号的意思就是嵌套，只是一般我们情况可以忽略它，但有时候需要使用

```css

.a{
    @nest .b &{
        color:red;
    }
}

/*结果*/

.b .a{
    color:red;
}

```
像上面这种特殊的，需要嵌套内部的做parent级时就需要使用到@nest了

#### 简单伪类嵌套

```css

.i{
  &:hover{
    font-size:12px;
  }
  &:not(0){
    color:red;
  }
}

```

结果为：

```css

.i:hover{

    font-size:12px;
}

.i:not(0){

    color:red;
}

```

这个没什么好说的，和选择器一样，都是拼接。


#### 复杂嵌套

```css

 .d,.e{
  &.f{
    font-size:12px;
  }
  
}

```

结果

```css

.d.f,.e.f{
    font-size:12px;
}


```

* 父选择器中存在`,`逗号分隔，这里就会分配匹配拼接,切记不是直接拼接哦

```css
.d,.e{
  &.f,&.g{
    font-size:12px;
  }
  
}
```
结果是:

```css
 .d.f,.d.g,.e.f,.e.g {

    font-size:12px;
}
```

* 这里父选择器和子选择器都有逗号分隔，这里也不是直接拼接，而是排列组合，
* 注意子选择器每次逗号分隔，都需要重新加上`&`符号。

在复制嵌套中其实也是可以使用@nest的，不过@nest得写在最前面，@nest表示该行是嵌套。

```css
.d,.e{
  @nest & .f , .i &{
    font-size:12px;
  }
}

/*结果是*/

.d .f, .i .d, .e .f, .i .e {
    font-size:12px;
}

/*不可以写成下面这样,@nest表示该行是嵌套*/

.d,.e{
   & .f ,@nest .i &{
    font-size:12px;
  }
}

```
* 嵌套不止嵌套两层，是可以嵌套多层的，你们自己试吧。

* 我们还可以结合自定义选择器一起使用，效果是一样的。

```css
@custom-selector :--test h1,h2,h3,h4;

:--test{
  color:red;
  & .a{
    font-size:12px;
  }
}
```
结果:

```css
h1,
h2,
h3,
h4{
  color:red
}

h1 .a,
h2 .a,
h3 .a,
h4 .a{
  font-size:12px;
}
```

### 媒体查询嵌套

```css
.a{
  color:red;
  @media (width >= 100px){
    color:blue;
    & .b{
      color:#f66;
    }
    @media (width <= 200px){
      color:yellow;
    }
  }
}
```
结果:

```css
.a{
  color:red;
}
@media (min-width: 100px){
  .a{
    color:blue;
  }
  .a .b{
    color:#f66;
  }
}
@media (min-width: 100px) and (max-width: 200px){
  .a{
    color:yellow;
  }
}
```

这个很好理解，我们一行一行分析，就很容易理解了

```css
.a{                             1
  color:red;                    2
  @media (width >= 100px){      3
    color:blue;                 4
    & .b{                       5
      color:#f66;               6
    }                           7
    @media (width <= 200px){    8
      color:yellow;             9
    }                           10
  }                             11
}                               12
```
```css

/*看到1和2，我们先写出*/

.a{
    color:red;
}

/*看到3,媒体查询是得写在最外面的吧*/

@media (min-width:100px){
    /*看到4,这个样式肯定属于.a的，所以有了*/
    .a{
        color:blue;
    }
    /*看到567，用我们选择器嵌套的思维来写就行*/
    .a .b{
        color:#f66;
    }
}

/*看到8行，因为这个媒体查询嵌套在另一个媒体查询里面的，肯定不能直接拼接，那就只能是范围结合了*/

@media (min-width:100px) and (max-width:20px){
    /*看到第9含，这个样式也是属于.a的*/
    .a{
        color:yellow;
    }
}

```

一行一行分析后是不是简单很多了。说真的，这么弄，如果复杂起来，看的人是真难受，逻辑复杂了，可读性也就低了。好吧，正常人也不会这么写。

哈哈，感觉这个嵌套没那么难，好像是我写复杂了~.~   ,好吧，我不认错， 下一篇我们接着学。

--------------------------------------------

## 参考文献

[http://cssnext.io/features/](http://cssnext.io/features/)