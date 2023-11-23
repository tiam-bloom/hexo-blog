---
title: Hexo基本安装和使用
cover: ''
tags:
  - hexo
  - 教程
categories: 教程
abbrlink: 148e19a2
date: 2023-01-23 21:46:02
---

# Hexo

hexo参考官方文档:

- https://hexo.io/zh-cn/docs/

butterfly主题配置 参考教程: 

- 推荐:  https://butterfly.js.org
- https://butterfly.zhheo.com/docs



## 全局安装

### 前提

```powershell
C:\Users\30362>git -v
git version 2.37.3.windows.1

C:\Users\30362>node -v
v18.12.1

C:\Users\30362>npm -v
8.19.2
```

### 安装

```powershell
C:\Users\30362>npm install -g hexo-cli

added 59 packages in 7s

C:\Users\30362>hexo -v
hexo-cli: 4.3.0
os: win32 10.0.19044
node: 18.12.1
v8: 10.2.154.15-node.12
uv: 1.43.0
zlib: 1.2.11
brotli: 1.0.9
ares: 1.18.1
modules: 108
nghttp2: 1.47.0
napi: 8
llhttp: 6.0.10
openssl: 3.0.7+quic
cldr: 41.0
icu: 71.1
tz: 2022b
unicode: 14.0
ngtcp2: 0.8.1
nghttp3: 0.7.0

C:\Users\30362>
```

## SSH连接

```shell
# 查看是否安装ssh
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo
$ ssh
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface]
           [-b bind_address] [-c cipher_spec] [-D [bind_address:]port]
           [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11]
           [-i identity_file] [-J [user@]host[:port]] [-L address]
           [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port]
           [-Q query_option] [-R address] [-S ctl_path] [-W host:port]
           [-w local_tun[:remote_tun]] destination [command [argument ...]]
```

```shell
# 生成秘钥
ssh-keygen -t rsa -C "github邮箱地址"
```

秘钥路径:`C:\Users\30362\.ssh\id_rsa.pub`

![image-20221118190321120](http://qiniu.yujing.fit/typora_img/image-20221118190321120.png)

```shell
# 测试是否连接成功
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo
$ ssh -T git@github.com
Hi tiam-bloom! You've successfully authenticated, but GitHub does not provide shell access.
```

## 初始化文件夹(仓库)

```shell
# clone hexo
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo init
INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git
error: RPC failed; curl 28 OpenSSL SSL_read: Connection was reset, errno 10054
fatal: expected 'packfile'
WARN  git clone failed. Copying data instead
INFO  Install dependencies
INFO  Start blogging with Hexo!
```

```shell
# 部署本地hexo blog, 开启hexo server
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo s
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
<Ctrl+C>
INFO  Have a nice day
```

![image-20221118194320476](http://qiniu.yujing.fit/typora_img/image-20221118194320476.png)

修改配置文件: `_config.yml`

```yml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  # 使用ssh连接更快
  repository: https://github.com/tiam-bloom/tiam-bloom.github.io.git 
  branch: main
```

```shell
# 安装hexo-deployer-git 自动部署发布工具
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ npm install hexo-deployer-git --save

added 1 package in 1s
```

```shell
# 上传文件到github仓库
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo g
INFO  Validating config
INFO  Start processing
INFO  Files loaded in 95 ms
INFO  Generated: archives/index.html
INFO  Generated: index.html
........
INFO  28 files generated in 343 ms
# 第二步
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo d
INFO  Validating config
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
...
remote: Resolving deltas: 100% (3/3), done.
To https://github.com/tiam-bloom/tiam-bloom.github.io.git
 + 97cc225...3e72f91 HEAD -> main (forced update)
branch 'master' set up to track 'https://github.com/tiam-bloom/tiam-bloom.github.io.git/main'.
INFO  Deploy done: git
```

![image-20221118193403425](http://qiniu.yujing.fit/typora_img/image-20221118193403425.png)

![image-20221118193526031](http://qiniu.yujing.fit/typora_img/image-20221118193526031.png)

## 结果

可以直接通过仓库名访问了

![image-20221118194200777](http://qiniu.yujing.fit/typora_img/image-20221118194200777.png)

![image-20221118194249569](http://qiniu.yujing.fit/typora_img/image-20221118194249569.png)

## 个性化自定义

- 清除缓存文件（db.json）和已生成的静态文件（public） => `hexo cl`
- `hexo g`
- `hexo s`
- `hexo d`

```shell
# 示列
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo cl
INFO  Validating config
INFO  Deleted database.
INFO  Deleted public folder.

30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo g
INFO  Validating config
INFO  Start processing
INFO  Files loaded in 130 ms
INFO  Generated: archives/index.html
INFO  Generated: archives/2022/11/index.html
INFO  Generated: archives/2022/index.html
......
INFO  28 files generated in 274 ms

30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo s
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
INFO  Farewell


30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo d
INFO  Validating config
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
....
 + 2340992...9d5fbcd HEAD -> main (forced update)
branch 'master' set up to track 'https://github.com/tiam-bloom/tiam-bloom.github.io.git/main'.
INFO  Deploy done: git
```



## 安装其他主题

```shell
# 在根目录下执行
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
Cloning into 'themes/butterfly'...
remote: Enumerating objects: 5600, done.
remote: Counting objects: 100% (207/207), done.
remote: Compressing objects: 100% (145/145), done.
remote: Total 5600 (delta 92), reused 144 (delta 62), pack-reused 5393
Receiving objects: 100% (5600/5600), 2.50 MiB | 1.17 MiB/s, done.
Resolving deltas: 100% (3660/3660), done.
```

```shell
# 安装pug 以及 stylus 的渲染器
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ npm install hexo-renderer-pug hexo-renderer-stylus --save


added 38 packages in 8s
```

```shell
# 解决文章中文路径失效问题, 安装插件
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ npm install hexo-abbrlink --save

added 2 packages in 3s
```

## 目录结构

![image-20230123214229198](http://qiniu.yujing.fit/typora_img/image-20230123214229198.png)

## 更新主题到服务器

```shell
30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo cl

30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo g

30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo s

30362@Tiam-Dell MINGW64 /d/wwwroot/Hexo-Blog
$ hexo d

```

主题自定义: http://haiyong.site/post/22e1d5da.html

域名: https://www.namesilo.com/

如何绑定; https://www.likecs.com/show-30474.html

官方主题: https://hexo.io/themes/

建建站文档: https://hexo.io/zh-cn/docs/setup

## 绑定域名

```powershell
C:\Users\30362>ping tiam-bloom.github.io

正在 Ping tiam-bloom.github.io [185.199.110.153] 具有 32 字节的数据:
来自 185.199.110.153 的回复: 字节=32 时间=191ms TTL=42
来自 185.199.110.153 的回复: 字节=32 时间=194ms TTL=42
来自 185.199.110.153 的回复: 字节=32 时间=200ms TTL=42
来自 185.199.110.153 的回复: 字节=32 时间=201ms TTL=42

185.199.110.153 的 Ping 统计信息:
    数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，
往返行程的估计时间(以毫秒为单位):
    最短 = 191ms，最长 = 201ms，平均 = 196ms
```

**IP:** 185.199.111.153



## 更新主题配置



## 评论

https://twikoo.js.org/

http://haiyong.site/post/17c68aa7.html

## 新建文章

```shell
hexo new "Title"
```

```powershell
source\_posts\Title.md
```

```shell
hexo g -d
```



## 本地图片位置

放在本地文件夹位置：`/themes/butterfly/source/img`

```yml
favicon: /img/favicon.png
```

## 基本使用

欢迎来到[Hexo](https://hexo.io/)！这是你的第一个帖子。查看[文档](https://hexo.io/docs/)了解更多信息。如果你在使用Hexo时遇到任何问题，你可以在[troubleshooting](https://hexo.io/docs/troubleshooting.html)上找到答案，或者你可以在[GitHub](https://github.com/hexojs/hexo/issues).上问我

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

# 常用命令

|        命令         |                             说明                             |
| :-----------------: | :----------------------------------------------------------: |
|      `hexo cl`      |       `cl`=>clear 清除缓存，当配置未生效的时候可以试试       |
| `hexo  new "title"` | 新建一个标题为`title`的文章，文件路径`/source/_posts/title.md` |
|      `hexo g`       |                `g => generate`，生成静态文件                 |
|      `hexo s`       | `s => server`，开启本地服务调试，适用经常修改配置查看效果时  |
|      `hexo d`       | `d => deploy`，部署到远程库，之后便可在外网访问了，通常会有一会延迟 |
|     `hexo g -d`     |         相当于`hexo g` + `hexo d`，发布新文章时可用          |



# 文章页配置

- 转自: https://butterfly.js.org/posts/dc584b87/#Post-Front-matter
- 页面配置见: https://butterfly.js.org/posts/dc584b87/#Page-Front-matter

```md
---
title:
date:
updated:
tags:
categories:
keywords:
description:
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
```

| 写法                  | 解释                                                         |
| --------------------- | ------------------------------------------------------------ |
| title                 | 【必需】文章标题                                             |
| date                  | 【必需】文章创建日期                                         |
| updated               | 【可选】文章更新日期                                         |
| tags                  | 【可选】文章标签                                             |
| categories            | 【可选】文章分类                                             |
| keywords              | 【可选】文章关键字                                           |
| description           | 【可选】文章描述                                             |
| top_img               | 【可选】文章顶部图片                                         |
| cover                 | 【可选】文章缩略图(如果没有设置top_img,文章页顶部将显示缩略图，可设为false/图片地址/留空) |
| comments              | 【可选】显示文章评论模块(默认 true)                          |
| toc                   | 【可选】显示文章TOC(默认为设置中toc的enable配置)             |
| toc_number            | 【可选】显示toc_number(默认为设置中toc的number配置)          |
| toc_style_simple      | 【可选】显示 toc 简洁模式                                    |
| copyright             | 【可选】显示文章版权模块(默认为设置中post_copyright的enable配置) |
| copyright_author      | 【可选】文章版权模块的`文章作者`                             |
| copyright_author_href | 【可选】文章版权模块的`文章作者`链接                         |
| copyright_url         | 【可选】文章版权模块的`文章连结`链接                         |
| copyright_info        | 【可选】文章版权模块的`版权声明`文字                         |
| mathjax               | 【可选】显示mathjax(当设置mathjax的per_page: false时，才需要配置，默认 false) |
| katex                 | 【可选】显示katex(当设置katex的per_page: false时，才需要配置，默认 false) |
| aplayer               | 【可选】在需要的页面加载aplayer的js和css,请参考文章下面的`音乐` 配置 |
| highlight_shrink      | 【可选】配置代码框是否展开(true/false)(默认为设置中highlight_shrink的配置) |
| aside                 | 【可选】显示侧边栏 (默认 true)                               |

# 其他链接

- [A-Obsidian](https://github.com/bennyxguo/hexo-theme-obsidian)
- Butterfly 
- https://korilin.github.io/hexo-theme-particle-demo/night/public/
- http://miccall.tech/
- https://volantis.js.org/
- [ParticleX](https://github.com/argvchs/hexo-theme-particlex)
- https://yelog.org/

