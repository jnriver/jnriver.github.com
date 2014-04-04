---
layout: post
category : programer
title : 打包带有第三方framework的app bundle
tags : [programer, cocoa]
---
{% include JB/setup %}

前些时候做了个查阅天气的app。开发测试过程中一路顺利，但是最后自己单拿app用时却总是秒crash…

查看mac的console：
> Dyld Error Message:
> Library not loaded: /Library/Frameworks/SBJson.framework/Versions/A/SBJson
> Referenced from: /Users/USER/Library/Developer/Xcode/DerivedData/Sunny_Dolls-exzudnitvcwzssewjlllarzktbrn/Build/Products/Debug/Sunny Dolls.app/Contents/MacOS/Sunny Dolls
> Reason: image not found

原来是我要用的SBJson.framework没找到。（开始想打包pkg安装，不知道能不能解决这个问题，不过由于证书的原因，pkg打包也不成功。）

展开app（右键-显示包内容），果然没有这个framework。于是在TARGETS的Build Phases中加一个Copy Files，类型自然是Frameworks，把SBJson.framework加进来。

<img src="/images/post/2013-04-02-AddFramework/1.png" width="640" >

重新build再试一次，还是不行，报一样的错。再看错误说明，当然不行了，只是把frameworks放在bundle里了，但是程序还是去/Library/Frameworks路径下找了。查了半天，在[文档](https://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Tasks/CreatingFrameworks.html)中看到:

<img src="/images/post/2013-04-02-AddFramework/2.png"/>
真是A记三行话，码农两行泪啊。。

根据第二条，果断把子项目SBJson的build setting中的`Installation Directory`改为`@loader_path/../Frameworks`

<img src="/images/post/2013-04-02-AddFramework/3.png"/>

搞定收工~

<img src="/images/post/2013-04-02-AddFramework/4.png"/>