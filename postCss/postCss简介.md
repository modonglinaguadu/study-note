# postCss简介

最近打算在部门中引入postCss，然后在网上找了很多学习资料，最全的是Kezz Bracey的postCss入门指南了，所以做了一点总结和补充。

## 简介

postCss现在在国内还未完全流行起来，现在大家用得比较多的也就autoprefixer,这个是postCss中其中一个插件。但我相信postCss会和pwa一样成为2018年的关键词。

> 那什么是postCss

在我看来postCss**仅仅**是一个平台，它就类似gulp，它把css转换成js的方式，然后提供一系列的处理css的方法，让大家参与进来处理css，编写各种插件。当然，现在它已经很完美了，有两百加的插件,你可以在[postCss插件列表](https://github.com/postcss/postcss/blob/master/docs/plugins.md)中看到，当然，未来它还会有更多。

> 那postCss可以结合以前的css预处理使用吗

当然可以，postCss仅仅是个平台，它有支持预处理的插件，当然我们也可以结合以前的css预处理器共同使用，现国内大多都是sass+autoprefixer模式，这不就是sass和postCss的结合吗。

## 总结

postCss就是一个平台，它有很多插件，每个插件都是一个js模块，你只需要把自己项目需要的模块加载进来即可，就像gulp一样，这使得它轻盈开放。


## 建议

现postCss的学习资料比较少，大多数入门参考资料都参考大神Kezz Bracey的[postCss入门指南](https://webdesign.tutsplus.com/series/postcss-deep-dive--cms-889),当然，如果觉得看英文吃力，国内大漠大神对其进行了翻译，你可以到[w3cplus](https://www.w3cplus.com/search/node/postcss)上查看，后面我的文章我主要以各个插件语法功能介绍为主，主要方式是翻译api文档和根据我的理解进行解释举例，较为乏味，不建议作为入门参考资料。

## 参考文献

[PostCSS Deep Dive: What You Need to Know](https://webdesign.tutsplus.com/tutorials/postcss-deep-dive-what-you-need-to-know--cms-24535)
