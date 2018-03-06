# postCss简介

## 简介

postCss现在在国内还未完全流行起来，现在大家用得比较多的也就autoprefixer,这个是postCss中其中一个插件。但我相信postCss会和pwa一样成为2018年的关键词。

> 那什么是postCss

在我看来postCss**仅仅**是一个平台，它就类似gulp，它把css转换成js的方式，然后提供一系列的处理css的方法，让大家参与进来处理css，编写各种插件。当然，现在它已经很完美了，有两百加的插件,你可以在[postCss插件列表](https://github.com/postcss/postcss/blob/master/docs/plugins.md)中看到，当然，未来它还会有更多。

> 那postCss可以结合以前的css预处理使用吗

当然可以，postCss仅仅是个平台，它有支持预处理的插件，当然我们也可以结合以前的css预处理器共同使用，现国内大多都是sass+autoprefixer模式，这不就是sass和postCss的结合吗。

## 总结

postCss就是一个平台，它有很多插件，每个插件都是一个js模块，你只需要把自己项目需要的模块加载进来即可，就像gulp一样，这使得它轻盈开放。