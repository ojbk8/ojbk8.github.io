---
title: "gitbook安装和插件的使用"
date: 2018-05-17T00:00:00+08:00
lastmod: 2018-05-17T00:00:00+08:00
draft: false
tags: ["gitbook"]
categories: ["教程"]

---


> gitbook安装使用插件教程

### 安装gitbook
先下载安装[Node.js](https://nodejs.org/zh-cn/),然后

``` bash
npm install -g gitbook-cli
gitbook init
gitbook install ./
gitbook build
gitbook serve
```

提示如下

``` bash
D:\gitbook>gitbook serve
Live reload server started on port: 35729
Press CTRL+C to quit ...

info: 7 plugins are installed
info: loading plugin "livereload"... OK
info: loading plugin "highlight"... OK
info: loading plugin "search"... OK
info: loading plugin "lunr"... OK
info: loading plugin "sharing"... OK
info: loading plugin "fontsettings"... OK
info: loading plugin "theme-default"... OK
info: found 5 pages
info: found 4 asset files
info: >> generation finished with success in 1.0s !

Starting server ...
Serving book on http://localhost:4000
```

你可以你的浏览器中打开这个网址： (http://localhost:4000)  预览


### 创建配置`book.json`

``` bash
{
    "title": "站点标题",
    "author": "作者",
    "description": "网站描述",
	"keywords": "关键字1,关键字2,keyword3",
    "language": "zh-hans",
    "gitbook": "3.2.3",
    "styles": {
        "website": "./styles/website.css"
    },
    "structure": {
        "readme": "README.md"
    },
	"links": {
        "sharing": {
            "all"      : true,
            "google"   : true,
            "facebook" : true,
            "twitter"  : true,
            "weibo"    : true,
            "qq"       : true,
            "qrcode"   : true
        }
    },
    "links": {
        "sidebar": {
            "链接网站": "https://github.com"
        }
    },
    "plugins": [
		"-livereload",
		"-highlight",
		"search",
		"-lunr",
		"sharing",
		"fontsettings",
		"theme-default",
		"copy-code-button",
		"anchor-navigation-ex",
		"donate"
    ],
    "pluginsConfig": {
        "anchor-navigation-ex": {
            "isRewritePageTitle": true,
            "isShowTocTitleIcon": true,
            "tocLevel1Icon": "fa fa-hand-o-right",
            "tocLevel2Icon": "fa fa-hand-o-right",
            "tocLevel3Icon": "fa fa-hand-o-right"
        },
		"donate": {
          "wechat": "/images/wechatpay.jpg",
          "alipay": "/images/alipay.jpg",
          "title": "",
          "button": "Donate",
          "alipayText": "支付宝捐赠",
          "wechatText": "微信捐赠"
        }

    }
	
}
```

### 配置`SUMMARY.md`示范演示

``` bash
# Summary

* [Introduction](README.md)
* [基本安装](howtouse/README.md)
   * [Node.js安装](howtouse/nodejsinstall.md)
   * [Gitbook安装](howtouse/gitbookinstall.md)
   * [Gitbook命令行速览](howtouse/gitbookcli.md)
```

使用Markdown语法创建编辑`xxx.md`

然后安装所需要的插件

``` bash
gitbook install
```

运行

``` bash
gitbook build
gitbook serve
```


