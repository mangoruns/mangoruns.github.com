---
layout: post
title: jekyll博客搭建(2)
category: jekyll
tagline: "build blog"
tags: [jekyll]
---
{% include JB/setup %}

## 头信息

头信息必须在文件的开始部分，并且需要按照 YAML 的格式写在两行三虚线之间。下面是一个基本的例子：

```
---
layout: post
title: Blogging Like a Hacker
---
```

你可以设置一些预定义的变量（下面这个例子可以作为参考）或者甚至创建一个你自己定义的变量。这样在接下来的文件和任意模板中或者在包含这些页面或博客的模板中都可以通过使用 Liquid 标签来访问这些变量。

> **UTF-8 编码方式警告**  
> 如果你使用UTF-8编码，那么在你的文件中一定不要出现 BOM 头字符，否则你会碰上非常糟糕的事情，尤其当你在Windows上使用Jekyll的时候。

> **提示™：头信息变量是可选的**  
> 如果你想使用 Liquid 标签和变量但是在头信息中又不需要任何定义，那么你可以将头信息设置为空！在头信息为空的情况下，Jekyll 仍然能够处理文件。（这对于一些像 CSS 和 RSS 的文件非常有用）

### 预定义的全局变量

![](http://i.imgur.com/fV8Nddg.jpg)

### 自定义变量

在头信息中预先定义的任何变量都会在数据转换中通过 Liquid 模板被调用。例如，在头信息中你设置一个 title，然后就可以在你的模板中使用这个 title 变量来设置页面的 title属性 ：

```
<!DOCTYPE HTML>
<html>
  <head>
    <title>{{ page.title }}</title>
  </head>
  <body>
    ...
```

### 在文章中预定义的变量

在文章中可以使用这些在头信息变量列表中未包含的变量。


变量名称	 |	描述
---------|-----------
date	 |	这里的日期会覆盖文章名字中的日期。这样就可以用来确定文章分类的正确

## 撰写博客

### 文章文件夹

所有的文章都在`_posts`文件夹中。 这些文件可以用`Markdown` 编写， 也可以用`Textile` 格式编写。只要文件中有 `YAML`头信息，它们就会从源格式转化成 HTML 页面，从而成为 你的静态网站的一部分。

#### 创建文章的文件

Jekyll 要求一篇文章的文件名遵循下面的格式：

    年-月-日-标题.MARKUP

在这里，年是4位数字，月和日都是2位数字。MARKUP扩展名代表了这篇文章 是用什么格式写的。下面是一些合法的文件名的例子：
	
	2011-12-31-new-years-eve-is-awesome.md
	2012-09-12-how-to-write-a-blog.textile

#### 内容格式

文章顶部必须有一段YAML头信息(YAML front- matter)。 在它下面，就可以选择你喜欢的格式来写文章。Jekyll支持2种流行的标记语言格式： Markdown 和 Textile. 

### 引用图片和其它资源

一种常用做法是在工程的根目录下 创建一个文件夹，命名为`assets` 或者 `downloads`，将图片文件，下载文件或者其它的 资源放到这个文件夹下。然后在任何一篇文章中，它们都可以用站点的根目录来进行引用。 这和你站点的域名/二级域名和目录的设置相关，下面有一些例子（Markdown格式） 来演示怎样利用`site.url`变量来解决这个问题。

在文章中引用一个图片
    
    … 从下面的截图可以看到：
	![有帮助的截图]({{ site.url }}/assets/screenshot.jpg)

链接一个读者可下载的 PDF 文件：

    … 你可以直接 [下载 PDF]({{ site.url }}/assets/mydoc.pdf).

> **提示™: 链接只使用站点的根URL**  
> 如果你确信你的站点只在域名的根 URL 下做展示，你可以不使用  {{ site.url }}变量。在这种情况下， 直接使用/path/file.jpg即可。

### 文章的目录

如何创建文章列表的简单例子：
```
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
```

### 文章摘要

Jekyll 会自动取每篇文章从开头到第一次出现`excerpt_separator`的地方作为文章的摘要， 并将此内容保存到变量`post.excerpt`中。

你可能想在每个标题下给出文章内容的提示，你可以在每篇文章 的第一段加上如下的代码：

```
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <p>{{ post.excerpt }}</p>
    </li>
  {% endfor %}
</ul>```

如果你不喜欢自动生成摘要，你可以在文章的`YAML`中增加`excerpt`来覆盖它。完全禁止掉可以 将`excerpt_separator`设置成`""`.

### 高亮代码片段

Jekyll 自带语法高亮功能，它是由 Pygments 来实现的。在文章中插入一段高亮代码非常 容易，只需使用下面的 Liquid 标记：

```
{% highlight ruby %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}
```

将输出下面的效果：

```
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
```
> **提示™：显示行数**  
> 你可以在代码片段中增加关键字linenos来显示行数。 这样完整的高亮开始标记将会是:{% highlight ruby linenos %}。

## 使用草稿

草稿是没有日期的文章。它们是你还在创作中而暂时不想发表的文章。想要开始使用草稿，你需要在网站根目录下创建一个名为 _drafts 的文件夹（如在目录结构章节里描述的），并新建你的第一份草稿：

```
|-- _drafts/
|   |-- a-draft-post.md
```

为了预览你拥有草稿的网站，运行 `jekyll serve` 或者带有 `--drafts` 配置选项的 `jekyll build`。此两种方法皆会将 `Time.now` 的值赋予草稿文章，作为其发布日期，所以你将看到草稿文章作为最新文章被生成。

## 创建页面

### 主页

`index.html` 文件， 这个文件将被做为主页显示出来。

> **提示™: 在主页上使用布局**  
> 站点上任何 HTML 文件，包括主页，都可以使用布局和 include 中的内容一般共用的内容，如页面的 header 和 footer. 将合适的部分抽出放到布局中

### 其它的页面的位置

将 HTML 文件放在哪里取决于你想让它们如何工作。有两种方式可以创建页面：

+ 命名 HTML 文件：将命名好的为页面准备的 HTML 文件放在站点的根目录下。
+ 命名文件夹：在站点的根目录下为每一个页面创建一个文件夹，并把 index.html 文件放在每 个文件夹里。

这两种方法都可以工作（并且可以混合使用），它们唯一的区别就是访问的 URL 样式不同。

### 命名 HTML 文件

增加一个新页面的最简单方法就是把给 HTML 文件起一个适当的名字并放在根目录下。 一般来说，一个站点下通常会有：主页 (homepage), 关于 (about), 和一个联系 (contact) 页。根目录下的文件结构和对应生成的 URL 会是下面的样子：

```
.
|-- _config.yml
|-- _includes/
|-- _layouts/
|-- _posts/
|-- _site/
|-- about.html    # => http://yoursite.com/about.html
|-- index.html    # => http://yoursite.com/
└── contact.html  # => http://yoursite.com/contact.html
```

### 命名一个文件夹并包含一个 index.html 文件

有些人不喜欢在 URL 中显示文件的扩展名。用 Jekyll 达 到这种效果，你只需要为每个顶级页面创建一个文件夹，并包含一个 inex.html 文件。 这样，每个 URL 就将以文件夹的名字作为结尾，网站服务器会将对应的 index.html 展示给用户。下面是一个示例来展示这种结构的样子：

```
.
├── _config.yml
├── _includes/
├── _layouts/
├── _posts/
├── _site/
├── about/
|   └── index.html  # => http://yoursite.com/about/
├── contact/
|   └── index.html  # => http://yoursite.com/contact/
└── index.html      # => http://yoursite.com/
```

## 常用变量

### 全局(Global)变量

![](http://i.imgur.com/xoCMqnG.jpg)

### 全站(site)变量

![](http://i.imgur.com/EURFa0O.jpg)

### 页面(page)变量

![](http://i.imgur.com/uP2ww9M.jpg)

> **ProTip™: Use custom front-matter**  
> 任何你自定义的头文件信息都会在 page 中可用。 具体来说，如果你在一个 Page 的头文件中设置了 custom_css: true， 这个变量就可以这样被取到 page.custom_css。

### 分页器(Paginator)

![](http://image227-c.poco.cn/mypoco/myphoto/20140522/09/5566361620140522095926021.jpg?668x462_120)

> **分页器变量的可用性**  
> 这些变量仅在首页文件中可以，不过他们也会存在于子目录中，就像 /blog/index.html。

## 模板

### 过滤器

![](http://image227-c.poco.cn/mypoco/myphoto/20140522/10/556636162014052210034008.jpg?664x995_120)

### 标签

#### 引用

在多个地方引用一小代码片段，可以使用 include 标签。

    {% include footer.html %}

Jekyll 要求所有被引用的文件放在根目录的 _includes 文件夹，上述代码将把 `<source>/_includes/footer.html` 的内容包含进来。

你还可以传递参数：

    {% include footer.html param="value" %}

这些变量可以通过 Lquid 调用：

    {{ include.param }}

#### Code snippet highlighting

要使用 `Pygments` ，你必须安装 `Python` 并且在配置文件 中设置 `pygments` 为 `true` 。

使用代码高亮的例子如下：

```
{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}
```
`highlight` 的参数 (本例中的 ruby) 是识别所用语言

##### 行号

`highlight` 的第二个可选参数是 `linenos` ，使用了 `linenos`会强制在代码上加入行号。例如：

```
{% highlight ruby linenos %}
def foo
  puts 'foo'
end
{% endhighlight %}
```
##### 代码高亮的样式

要使用代码高亮，你还需要包含一个样式。例如你可以在 [syntax.css](https://github.com/mojombo/tpw/blob/master/css/syntax.css) 找到，这里有 跟 `GitHub` 一样的样式，并且免费。如果你使用了 `linenos` ，可能还需要在 `syntax.css `加入 `.lineno` 样式。

#### Post URL

如果你想使用你某篇文章的链接，标签 `post_url` 可以满足你的需求。

	{% post_url 2010-07-21-name-of-post %}

当使用`post_url`标签时，不需要写文件后缀名。

还可以用 Markdown 这样为你的文章生成超链接：

	[Name of Link]({% post_url 2010-07-21-name-of-post %})

Gist

使用 `gist` 标签可以轻松的把 GitHub Gist 签入到网站中：

	{% gist 5555251 %}

你还可以配置 gist 的文件名，用以显示：

	{% gist 5555251 result.md %}

`gist` 同样支持私有的 gists ，这需要 gist 所属的 github 用户名：

	{% gist parkr/931c1c8d465a04042403 %}

私有的 gist 同样支持文件名。

## 永久链接

你可以通过 `Configuration` 或 `YAML` 头信息 为每篇文章设置永久链接。你可以随心所欲的选择你自己的 格式，即使自定义。默认配置为 `date`。

永久链接的模板用以冒号为前缀的关键词标记动态内容，比如 `date` 代表 `/:categories/:year/:month/:day/:title.html` 。

### 模板变量

![](http://i.imgur.com/Pa8EkUR.jpg)

### 已经建好的链接类型

链接类型	| URL 模板
--------|-----------
date	|	/:categories/:year/:month/:day/:title.html
pretty	|	/:categories/:year/:month/:day/:title/
none	|	/:categories/:title.html

### 举例

比如文件名： /2009-04-29-slap-chop.textile

设置		|	对应的 URL
--------|---------------
没有配置或 permalink: date	|	/2009/04/29/slap-chop.html
permalink: pretty			|	/2009/04/29/slap-chop/index.html
permalink: /:month-:day-:year/:title.html	|	/04-29-2009/slap-chop.html
permalink: /blog/:year/:month/:day/:title	|	/blog/2009/04/29/slap-chop/index.html

## 分页功能

> **分页功能只支持 HTML 文件**  
> Jekyll 的分页功能不支持 Markdown 或 Textile 文件，而是只支持 HTML 文件。当然，这不会让 你不爽。

### 开启分页功能

开启分页功能很简单，只需要在 `_config.yml`里边加一行，并填写每页需要几行：

	paginate: 5

下边是对需要带有分页页面的配置：

	paginate_path: "blog/page:num"

`blog/index.html`将会读取这个设置，把他传给每个分页页面，然后从第 2 页开始输出到 `blog/page:num` ， `:num` 是页码。如果有 12 篇文章并且做如下配置 `paginate: 5` ， Jekyll会将前 5 篇文章写入 `blog/index.html` ，把接下来的 5 篇文章写入 `blog/page2/index.html`，最后 2 篇写入 `blog/page3/index.html`。

### 与 paginator 相同的属性

![](http://image227-c.poco.cn/mypoco/myphoto/20140522/11/5566361620140522110633034.jpg?666x464_120)

> **不支持对“标签”和“类别”分页**  
> 分页功能仅仅遍历文章列表并计算出结果，并无读取 YAML 头信息，现在不支持对“标签”和“类别”分页。

### 生成带分页功能的文章

接下来要做的事情就是展现在页面上了，下边是一个简单的例子：

```
---
layout: default
title: My Blog
---

<!-- 遍历分页后的文章 -->
{% for post in paginator.posts %}
  <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
  <p class="author">
    <span class="date">{{ post.date }}</span>
  </p>
  <div class="content">
    {{ post.content }}
  </div>
{% endfor %}

<!-- 分页链接 -->
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="/page{{ paginator.previous_page }}" class="previous">Previous</a>
  {% else %}
    <span class="previous">Previous</span>
  {% endif %}
  <span class="page_number ">Page: {{ paginator.page }} of {{ paginator.total_pages }}</span>
  {% if paginator.next_page %}
    <a href="/page{{ paginator.next_page }}" class="next">Next</a>
  {% else %}
    <span class="next ">Next</span>
  {% endif %}
</div>
```
> **注意首尾页**  
> Jekyll 没有生成文件夹 ‘page1’ ，所以上边的代码有 bug ，下边的代码解决了这个问题。

下边的 HTML 片段是第一页，他除自己外，为每个页面生成了链接。

```
<div id="post-pagination" class="pagination">
  {% if paginator.previous_page %}
    <p class="previous">
      {% if paginator.previous_page == 1 %}
        <a href="/">Previous</a>
      {% else %}
        <a href="{{ paginator.previous_page_path }}">Previous</a>
      {% endif %}
    </p>
  {% else %}
    <p class="previous disabled">
      <span>Previous</span>
    </p>
  {% endif %}

  <ul class="pages">
    <li class="page">
      {% if paginator.page == 1 %}
        <span class="current-page">1</span>
      {% else %}
        <a href="/">1</a>
      {% endif %}
    </li>

    {% for count in (2..paginator.total_pages) %}
      <li class="page">
        {% if count == paginator.page %}
          <span class="current-page">{{ count }}</span>
        {% else %}
          <a href="/page{{ count }}">{{ count }}</a>
        {% endif %}
      </li>
    {% endfor %}
  </ul>

  {% if paginator.next_page %}
    <p class="next">
      <a href="{{ paginator.next_page_path }}">Next</a>
    </p>
  {% else %}
    <p class="next disabled">
      <span>Next</span>
    </p>
  {% endif %}

</div>
```
