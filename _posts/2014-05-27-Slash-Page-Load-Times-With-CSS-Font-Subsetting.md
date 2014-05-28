---
layout: post
title: 用CSS字体构造子集降低页面加载时间
category: translate
tagline: "Slash Page Load Time"
tags: [fontface]
---
{% include JB/setup %}

网页字体增加了页面大小，花费大量网页加载时间，注重性能的开发者已经开始[优化他们的图片](http://demosthenes.info/blog/550/Site-Optimisation-Ultimate-Image-Compression-Tools)和[CSS](http://demosthenes.info/blog/686/CSS-Compression-Workflow)，优化网页字体对于我们来说也是有意义的

一个好的消息是这个很容易：创建你的自定义字体或者为你的网页字体主机服务器提供一个简单的变量值，**你可以只加载你页面上所需要的文本的字体符号而不是字体中全部的字符**，数字和图形，来减少页面加载时间

字体构造子集非常有效，只要你记住两个条件:  

+ 为了使用这种技术的最有效的版本——从字体中加载一个有限范围的字符集——你应该确定字体渲染的文本**在将来是稳定不会改变的**，比如没有加载的字符是不会正确地显示的  

+ 如果你自托管网页的字体,你应该首先确保`non-WOFF`字体是被压缩的,这将节省平均文件大小40 ~ 70%,在你将注意力转移到从构造子集获得优势之前。

谷歌字体构造子集
--------------
最流行的网页字体主机服务商有大量的子集的选项，第一个已经内置了

![](http://demosthenes.info/assets/images/google-character-sets.png)
谷歌字体默认选择的字符集

默认情况下,网站只会下载拉丁字符子集(大写和小写字母A - Z、数字和标点符号)。“扩展”拉丁字符子集的选择应该包含额外的字符用于其他欧洲语言:重音字符,变音符号,等,但这些字符的质量和由谷歌提供的这些字符范围通常是有限的:你可能,在某些情况下,想要为自己的字体获得完整的字符集。

构造子集的选择也会改变字体请求的URL:

    <link href="http://fonts.googleapis.com/css?family=Open+Sans&subset=latin-ext,latin" rel="stylesheet">

**限制谷歌字体为特定字符**
你也可以限制请求你需要使用的文本的字符的URL变量:

    <link href="http://fonts.googleapis.com/css?family=Open+Sans&text=Hello" rel="stylesheet">

这只为“H”、“e”,“l”和“o”开放谷歌的`Open Sans`字符集,显著减少请求的字体大小:

**完全请求（无构造子集）**

    <link href="http://fonts.googleapis.com/css?family=Open+Sans" rel="stylesheet">

![](http://demosthenes.info/assets/images/google-font-request.png)
14.7 kb下载字体,完整的页面渲染时间超过100 ms

**请求子集**

    <link href="http://fonts.googleapis.com/css?family=Open+Sans&text=Hello" rel="stylesheet">

![](http://demosthenes.info/assets/images/google-font-request-subset.png)
1.2 kb字体下载，页面渲染时间约60 ms。

这种技术非常适合网页字体用于呈现标识或设置标题,使用的字符总是会提前知道。请注意,如果你的文本包含的字符超出了规定的范围您可能会遇到麻烦:

{% highlight html %}
<link href="http://fonts.googleapis.com/css?family=Open+Sans&text=Hello" rel="stylesheet">
<style>
h1 { font-family: Open Sans; }
</style>
<h1>Hello, Frendo</h1>
{% endhighlight %}

![](http://demosthenes.info/assets/images/hello-frendo.png)  

你可以看到“F”。“r”、“n”和“d”字符不正确地呈现,他们没有在我们最初的请求中指定。

大多数其他的字体托管服务,比如[fonts.com](http://fontsubsetter.com/Textdemo/Latin),提供这种技术的变化,你应该参考其文档以了解更多。

将重复字符添加到请求字符串中是多余的,我已经创建了一个快速的JavaScript工具,将从任何字符或短语中显示独特的字符串,要求按字母顺序,以帮助您构建自己的请求。

自托管Web字体的构造子集
---------------------
你还可以从您自己的服务器字体子集里制造一个定制的字体,只包含你想要的符号。
> 通常(可以理解为)假定CSS `unicode-range`属性为字体的子集。这只是部分正确:在`Unicode-range`整个字体仍然加载,但只有一组的字符集在实际使用的范围。因此,它不是一个我们在这里讨论的网页字体优化技术,并且不节省下载时间。

最简单的方法,目前是通过使用[Font Squirrel webfont generator service](http://www.fontsquirrel.com/tools/webfont-generator)的专家级选项

![](http://demosthenes.info/assets/images/font-squirrel-subsetting.png)

> 很明显,你必须保证你可以以这种方式操作字体:先阅读许可协议。

在这种情况下,我决定,我只想从Lobster字体中使用大写字母。提取这些网页字体成为比原来更小的文件:

lobster.woff		62KB  

lobster-caps.woff	7.7KB

我可以保证,大写字母只会用Lobster字体排版在我的网站上，因此我的CSS:

    @font-face { font-family: Lobster; src: url("Lobster/lobster-caps.woff"); }
	h1 { font-family: Lobster; text-transform: uppercase; }

如果你只使用字体的几个字符,它可以使用base-64来编码结果,拯救一个HTTP请求;用SVG呈现独特的符号也是一种选择。

结论
----
构造你的字体子集可以是一个非常强大的和有用的工具,但需要仔细考虑和计划。设计师的需求、内容提供商和翻译员都应该考虑选择在什么文本范围中使用一个字体子集。

