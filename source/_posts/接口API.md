---
title: 分享一些接口
tags:
  - API
categories:
  - API
keywords: API
description: 分享一些接口
abbrlink: 7c551d72
date: 2023-02-08 13:17:42
updated:
top_img:
comments:
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---

# 接口API

# GitHub

- 文档 https://docs.github.com/en/rest?apiVersion=2022-11-28
- 接口 https://api.github.com/
- 用户信息 https://api.github.com/users/tiam-bloom
- 查询 https://api.github.com/search/users?q=tiam-bloom

# 网易音乐

- 在线项目实例: https://smallruraldog.github.io/vue3-music/#/discover



- 文档 https://neteasecloudmusicapi.vercel.app/#/
- https://autumnfish.cn
- 热搜 https://autumnfish.cn/search/hot    https://autumnfish.cn/search/hot/detail
- 歌单 https://autumnfish.cn/personalized
- 榜单 https://autumnfish.cn/toplist
- 喜欢列表 https://autumnfish.cn/likelist?uid=32953014
- 搜索歌手(根据id)  https://autumnfish.cn/artist/top/song?id=7763
- 搜索 https://autumnfish.cn/cloudsearch?keywords=邓紫棋

![image-20221205130123855](http://qiniu.yujing.fit/typora_img/image-20221205130123855.png)

- 获取歌词 https://autumnfish.cn/lyric?id=30612793
- 获取歌曲url https://autumnfish.cn/song/url?id=30612793

# 博客园

- 文档 https://api.cnblogs.com/help

# 微博

- https://open.weibo.com/wiki/API



# 社会语录

- http://api.uixsj.cn/hitokoto/get?type=social
- 文档 https://api.uixsj.cn/hitokoto/index.html  

| 名称 | 必填 | 类型   | 说明                                                         |
| :--- | :--- | :----- | :----------------------------------------------------------- |
| type | 否   | string | 选择输出分类[hitokoto\|en\|social\|soup\|fart\|zha]，为空默认hitokoto |
| code | 否   | string | 选择输出格式[json\|js]                                       |

hitokoto(一言)、en(中英文)、social(社会语录)、soup(毒鸡汤)、fart(彩虹屁)、zha(渣男语录)

# 大西瓜

https://api.no0a.cn/



# 爬取icon

```
思路: 

第一种方法
如果想要获取一个网站的页面logo，最简单且最有效的办法就是在它的网址后面加上/favicon.ico
例如： https://www.baidu.com/favicon.ico
就可以获取了！

第二种方法
在浏览器界面按下F12，打开开发者模式，在 Elements中打开<head>...</head>标签
找到<link....> 中含有favicon或ico的链接，复制即可
因为图标是放在head部分的link中，通过设置rel="icon"或者 rel="shortcut icon" 来让浏览器识别
```

一

API:  https://api.iowen.cn/doc/favicon.html

```
https://api.iowen.cn/favicon/www.yujing.fit.png
```

==注意:==不需要 http(s):// ，且结尾必须填 .png

二

https://favicongrabber.com/

```
http://favicongrabber.com/api/grab/github.com
```

三

```
www.google.com/s2/favicons?domain=https://yujing.fit/
```

# 爬取标题

```
https://api.no0a.cn/api/sitetitle/query?url=https://yujing.fit
```

```json
{"status":1,"title":"J's Blog"}
```

```js
async getTitles(url) {
      // 获取网站标题
      return await fetch(url)
        .then((res) => res.text())
        .then((html) => {
          const title = html.match(/<title>(.*?)<\/title>/);
          console.log(title);
          return title ? title[1] : "";
        });
    },
```







