---
title: Node-Ping-VPS
comments: false
date: 2017-04-10 20:11:44
tags:
- 有趣的作品
- Node-Ping-VPS
categories:
- 有趣的作品
- Node-Ping-VPS
---

## 简介

工作学习娱乐离不开ss，但ss偶尔会因为某个服务器地址暂时失效而挂掉，用Node.js写了个批量ping服务器地址的小程序，蛮实用的。

## 总览

{% asset_img intro.gif %}

<!-- more -->

<br/>

主要技术：
- Node.js
- AngularJS
- Express

CSS framework
- [Materialize CSS](http://materializecss.com/)

Third-party lib
- ping

## 警告 !

因为第三方库 `ping` 不支持 `gbk` 等编码，故在中文版Windows上无法正常使用该库。因此我修改了第三方库 `ping` 部分源码，导致整个 `node_modules\` 文件夹都必须 push 上去，所以 clone 该项目后**绝对不可以**使用命令 `npm install`

被修改文件的位置：`node-ping-vps\node_modules\ping\lib\ping-promise.js`

修改的代码如下，返回结果的内容修改为了**十六进制文本**

```javascript
ls.stdout.on('data', function (data) {
    // @modified by hugohoo
    // outstring += String(data);
    outstring += data.toString("hex");
});

ls.on('close', function (code) {
        var result;
        var time;
        // @modified by hugohoo
        // var lines = outstring.split('\n');
        var lines = new Buffer(outstring, "hex").toString().split("\n");

        // workaround for windows machines
        // if host is unreachable ping will return
        // a successfull error code
        // so we need to handle this ourself
        if (p.match(/^win/)) {
            // this is my solution on Chinese Windows8 64bit
            result = false;
            for (var i = 0; i < lines.length; i++) {
                var line = lines[i];
                if (line.search(/TTL=[0-9]+/i) > 0) {
                    result = true;
                    break;
                }
            }

            // below is not working on My Chinese Windows8 64bit
            /*
            for (var t = 0; t < lines.length; t++) {
                if (lines[t].match (/[0-9]:/)) {
                    result = (lines[t].indexOf ("=") != -1);
                    break;
                }
            }
            */
        } else {
            result = code === 0;
        }

        for (var t = 0; t < lines.length; t++) {
            var match = /time=([0-9\.]+)\s*ms/i.exec(lines[t]);
            if (match) {
                time = parseFloat(match[1], 10);
                break;
            }
        }
        deferred.resolve({
            host: addr,
            alive: result,
            output: outstring,
            time: time,
        });
    });
```

## 添加服务器列表文件

文件格式要求为 `json`

文件内文本格式如下：

```
[
    "hk01.ipip.pm",
    "hk02.ipip.pm",
    "hk03.ipip.pm",
    "hk04.ipip.pm",
    "kr01.ipip.pm",
    "kr02.ipip.pm",
    "us01.ipip.pm",
    "us02.ipip.pm"
]
```

文件放入 `node-ping-vps\public\fls\` 文件夹下即可

## 打包成桌面应用

使用 [NW.js](https://nwjs.io/) 可以把该项目打包成桌面应用，同时需要在 `node-ping-vps\app.js` 的最后添加几代码即可。

```javascript
nw.Window.open("http://localhost:3000", {}, function(win){
    win.y = 100;
    win.height = 700;
});
```

效果：

{% asset_img intro02.gif %}

## 安装

```
git clone https://github.com/HugHoo/node-ping-vps.git

cd node-ping-vps

node app.js
```