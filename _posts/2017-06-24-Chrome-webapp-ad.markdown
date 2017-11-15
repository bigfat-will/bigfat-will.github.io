---
layout:     post
title:      "简单的Chrome扩展程序"
subtitle:   ""
date:       2017-06-24
author:     "Will"
header-img: "img/google/chrome-ad00.png"
catalog: true
tags:
    - Google
---

> “walk beside you ”


## 前言 

　　有思念你的人在的地方，就是你的归处。
                                ------  《火影忍者》

---

## 正文

　　平时的生活、工作中，我们浏览的网页少不了伴随着广告，虽然不影响使用，但是碍眼啊。所以就想着弄个插件来过滤下，
于是拿百度先练手。Chrome的插件开发其实很简单，会一些前端的东西就可以开发，只是刚开始不知如何下手。然后在网上找了个[360的文档。。。](http://open.chrome.360.cn/extension_dev/overview.html)
摸索着过河。
![baidu](/img/google/chrome-ad01.png)
## Chrome插件

Chrome的插件文档上说应该包含以下文件：
   * manifest.json（必须）
   * 一个或多个Html
   * 一个或多个Js
   * 图片、图标

#### 插件
   **manifest.json** 这个Json文件主要是一些插件的信息，比如：

   ```
    {
        "name": "页面广告清除",
        "manifest_version": 2,
        "version": "1.0",
        "description": "暂只清除百度广告~ blog.smallfat.cn",
        "browser_action": {
            "default_icon": "icon128.png"
        },
        "icons": {
            "16": "icon16.png",
            "48": "icon48.png",
            "128": "icon128.png"
        },
        "permissions": [
            "*://*.baidu.com/*"
        ],
        "content_scripts": [
            {
                "matches": [
                    "*://www.baidu.com/*"
                ],
                "css": [
                    "main.css"
                ]
            }
        ]
    }
   ```

   * **name** 插件的名称
   * **manifest_version** manifest文件自身格式的版本号,现在强制为 2 ，不然会报错
   * **version** 当前插件的版本
   * **description** 描述
   * **browser_action** 控制插件图标的属性，如 default_icon 为 显示的图标
   * **icons** 一个或者多个图标，在安装的时候显示使用，需要提供三种尺寸
   * **permissions** 插件使用的权限，这里写的是百度的网址，意思是只要域名是baidu.com的时候才有权限
   * **content_scripts** 在文本页面运行的脚本文件，这里matches的意思是当前URL与之匹配的时候，下面的脚本生效


　　这里只写了我用到的一些参数，还有好多参数没有列出来，可以再上门提供的链接里查到。

　　从上面可以看出引用的图片、css 使用的路径为同一目录。所以我们需要创建一个目录，把所需要的
   **manifest.json**、**图片**、**css** 放进去。

　　我们要写的是一个去除搜索页面中 是广告的 搜索结果，所以可以用js来吧这些内容去掉，或者用css 把这样内容隐藏。
我这里用css来实现。

　　通过查看Html元素，我们发现广告都是在 **id="content_left"** 中 而且 样式都为 **style="display:block !important;**
   ![content_left](/img/google/chrome-ad02.png)

　　有于上面使用了!important you'xian'优先级最高，所以我们不能使用display，我们这里使用
   ```
    #content_left{ position: relative; }
    /*为你推荐*/
    #content_left .leftBlock{display: none !important;}
    /*广告*/
    #content_left > div[style^="display"]{
        position: absolute;
        top: -1000px;
    }

   ```

　　我们把写好的css、修剪好尺寸的图片、manifest.json 放到一个文件下。我们使用Chrome浏览器 ```chrome://extensions/``` 中的添加已解压的扩展程序来验证我们的插件，
如果我们的manifest.json写的有问题，则这里会显示错误。


#### 打包

　　Chrome的插件都是以crx 为后缀的。我们可以使用Chrome浏览器 ```chrome://extensions/``` 中的打包扩展程序进行打包。
打包成功后会生产两个文件：
   * 一个是crx为后缀的插件
   * 一个是pem为后缀的私钥文件,用于以后版本升级使用

---

## 后记

　　现在我们可以把我们打好包的插件直接拖拽到Chrome中，进行安装。以后百度搜索的时候就看不到碍眼的广告了~

　　和过滤百度广告一个道理，我们可以扩展这个插件使其可以过滤CSDN、开源等网页的广告~

   [源码和插件](http://pan.baidu.com/s/1o78UHaa)

　　

