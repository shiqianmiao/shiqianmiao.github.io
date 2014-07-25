---
layout: post
title: "IOS下使用Storage时的一个坑"
categories: [JavaScript]
tags: [Javascript]
---
{% include codepiano/setup %}

昨天同事告诉我公司的M站在 IPhone 手机上的Javascript全部都无法执行，让我帮忙看看出了什么问题。  
听到在IPhone上出问题也是见怪不怪了，把手机连接上电脑调试了下发现报了如下错误：

    QuotaExceededError: Dom exception 22: An attempt was made to add something to storage that exceeded the quota

意思是Storage超出了配额，我只想说，你是在开玩笑嘛，公司的M站只在storage初始化的时候添加了一条key/value而已啊。  
我的直觉告诉我这肯定又是IPhone上坑爹的设计（bug）中的一个，果不其然，我在stackoverflow上找到了这么一个回答：
[stackoverflow](http://stackoverflow.com/questions/21159301/quotaexceedederror-dom-exception-22-an-attempt-was-made-to-add-something-to-st)。

原来Safari在所谓的private mode下不允许使用Storage功能，只有在用户自身开启non-private mode的情况下才可以正常使用Storage。  
所谓的non-private mode就是中文safari下的无痕模式，敢问正常的用户有几个人会去开启这个模式？
既然不让人使用就不要把Storage开放出来啊。。。囧死个人，没办法只好我们自己去判断Storage功能是否可用了。  
正常情况下我们会这么判断Storage是否可用：

    if ( !window.localStorage ) {
        //ie6，ie7 使用UserData替代实现
        //...
    } else {
        //使用storage
    }

可惜的是这种判断方法在safari上不适用，localStroage，sessionStroage和Storage在safari上都是存在的，只是不让使用而已（就不让你用，来打我啊~O(∩_∩)O哈哈~）
不过因为在使用的时候会报QuotaExceedError，利用这一点我们可以通过try/catch来判断。代码如下：

    if ( !window.localStorage ) {
        //ie6，ie7 使用UserData替代实现
        //...
    } else {
        //判断是否Storage是否真的可用
        var localStorage = window.localStorage;
        var test = 'test';
            try {
                storage.setItem(test, '1');
                storage.removeItem(test);
                //使用Storage
            } catch (e) {
                //使用cookie替代实现
            }
    }

这里就不把全部代码写下来了，因为大家的Storage封装可能都不太一样。

不得不说不管是在开发Wap还是Web App，IPhone上的坑都是多的惊人，明明在android上能完美运行的代码在IPhone上就是出各种莫名其妙问题，果然我是没办法喜欢上IPhone啊。  

PS:关于Web App开发能使用的UI框架，最近百度的团队正要开源一个Web App的框架blend UI，在演示现场运行的时候确实有着不错的效果。  
经常要做Web App的人可以关注一下blend UI的开源进度，相信肯定会对提高大家在开发Web App时的效率上有一定的帮助。

