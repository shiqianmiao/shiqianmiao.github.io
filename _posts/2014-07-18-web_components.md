---
layout: post
title: "Web Components -- Web组件标准化的未来"
categories: [HTML]
tags: [Javascript, HTML]
---
{% include codepiano/setup %}

7月17日google发布了Chrome 36正式版，这个版本正式支持了HTML imports，这意味着Web Components的全面实现已经不再是遥远的未来了。  
身为一个Web开发者怎么不來体验一下这些面向未来的组件标准呢？果断升级了Chrome 36，抢先体验一把。

如果还不是很了解什么是Web Components的话，可以看看[components-introduction](http://www.w3.org/TR/components-intro/)。

###Templates
Chrome 26开始支持template标签，这个标签可以看做是客户端的模板，我们在服务器端和客户端的模板引擎层出不穷，想必大家也都用过不少了，两边各有特点。

####服务器端模板引擎
在服务器端将模板和数据合成，返回最终的HTML页面  

#####优点
1. 渲染过程不执行JavaScript脚本？
2. SEO友好

#####缺点
1. 客户端模板的优点就是它的缺点

####客户端模板引擎
将模板和数据分别传送到客户端，在客户端由JavaScript模板引擎渲染出最终的HTML视图。  

#####优点
1. 可以将静态的模板文件进行缓存和负载均衡，对于流量大的网站很有价值。
2. 模板渲染过程转移到客户端，减轻服务器的负荷。
3. 可复用性高。一个页面的前端内容可能来自不同后台，用了不同的架构，甚至是不同的语言。他们的模板引擎可能五花八门根本不能复用共享。  
客户端不存在这个问题，不同的架构，不同的语言可以使用相同的客户端渲染引擎，从而实现模板共用，结构统一，易于修改和扩展。
4. 从职责区分来说，模板渲染其实算是前端开发的工作。长久以来后端工程师一直苦于需要在处理数据的同时还要参与页面上的展示逻辑。
前后端的工作一直紧紧的耦合在一起，使用客户端模板从一定程度上解决了这个问题。  
后端工程师只需要专注于底层业务逻辑和数据的处理，而前端工程师也不需要再和后端工程师在HTML结构上苦苦纠结。

#####缺点
1. SEO不友好？

我们再来看看template，它不但拥有上述客户端模板引擎的所有优点，最重要的是它是Web Components的一个标准，
这意味着未来的某一天，所有主流的浏览器都可以使用它来做模板引擎而不用下来多余的模板引擎框架文件，不需要担心找不到会使用它的人！

上面我打上了问号（？）的地方其实算是一种讹传，他们在某些搜索引擎下（某度）的时候是成立的，但Google是可以执行Javascript脚本的，所以不存在SEO不友好的问题。
技术都是一直向前进步的，不是吗？

####正式上场之前

在我们正式使用前，先写个小函数来判断你的浏览器是否支持tamplate。

    function supportsTemplate() {
       return 'content' in document.createElement('template');
    }
     
    if (supportsTemplate()) {
       //支持
    } else {
       //不支持
    }

接下来实现一个小例子。
<a class="jsbin-embed" href="http://jsbin.com/qiviq/4/embed?html,js,output">JS Bin</a>

&lt;template&gt;标签中的所有元素都会由解析器解析，但仅仅只是解析。中间如果有脚本，脚本不会马上执行，图片也不会下载，而&lt;template&gt;本身也不会被显示出来。  
只有在把&lt;template&gt;中的元素复制插入或直接插入页面时，其中的内容才会被运行。
在上面的例子中就是这一句。

    document.body.appendChild(content.cloneNode(true));

可以想象他配合上 imports 可以很轻松实现按需加载js的功能，而且从也很符合逻辑，先加载对已模板需要的js再渲染模板，是不是很赞呢？

<script src="http://static.jsbin.com/js/embed.js"></script>