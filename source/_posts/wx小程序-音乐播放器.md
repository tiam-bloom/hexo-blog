---
title: WX小程序--音乐播放器
cover: >-
  http://qiniu.yujing.fit/image/nice/zentree_1.png
tags: wx小程序
id: '326'
categories:
  - - 小程序
abbrlink: ed803555
date: 2021-09-18 23:00:37
---

从最开始了解微信小程序到萌生想法想自己做一个发布到自己的公众号，中间还是隔了蛮久，近几天突然有了想法做什么了，今天却因为个人小程序的原因审核不通过，可气。

最开始也是一点也不了解，甚至小程序用什么语言写的也不知道。萌生了想法就去找[官方文档](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/)查看，寥寥草草的差不多看了一晚上，知道了小程序的文件结构以及所需的编程语言，主要编程语言是JavaScript，好在JavaScript有学过一点，至少还有点基础。还有新的语言，WXML和WXSS，不过呢WXML和HTML极为相似，WXSS和CSS极为相似，正好HTML和CSS都学过一点，还记得一点基本语法。也不算新的东西。还有个配置文件的JSON格式，虽不是编程语言，也没有学过，可是之前几次做项目却用过几次，大概还记得怎么用。就这样，觉得自己应该可以做出来，第二天就开始新建项目做了。

当然过程中会遇到很多很多的困难，只能一步一步慢慢解决，慢慢来了。

实践的时候，总觉得没有之前那么逻辑顺畅，而且有了之前的经验应该会该快更好才对。上学期用Vue映入API接口做了个网页的音乐播放器，然后又用uni-app做了个表格的增删改查。虽不一样，却有很多类似的地方可以借鉴。比如：页面间传递数据、将API获取到的数据渲染到页面上等等。

虽现在还没有做得很好，但是音乐播放的基本功能还是实现了，页面样式还没有写，丑得一批，可惜没有设计的头脑，目前的想法只有借鉴其他音乐播放器的样式排版了（虽然不一定能做得出来哈哈）。

好了，记录一下到完成到现在的功能的思路及实现。（目前2021/9/18）

* * *

* * *

先是音乐资源，刚好可以借用到上次Vue音乐播放器用的[网易云API](https://autumnfish.cn)，然后优先要实现最主要的基本功能，功能太少无实际作用，太多难以实现又复杂。那么初步定义的功能有什么呢？那必定要实现查询功能了，想听什么都可以查寻到，再是能让查询到歌曲能够播放出来（有声就行）。哈，就两个功能，感觉挺简单。

创建一个小程序的新项目，有一个默认的首页，就默认当作首页了，添加一个按钮，点击进入首页，就是搜索页面，输入搜索，将查询到的歌曲渲染到页面，点击歌曲进入播放页面，播放歌曲。

### 注册页面

所以需要在配置文件app.json中注册两个层级的页面，一个显示歌曲列表music，一个显示播放界面player。

![](http://yujing.fit/wp-content/uploads/2021/09/图片-58.png)

![](http://yujing.fit/wp-content/uploads/2021/09/图片-57.png)

![](http://yujing.fit/wp-content/uploads/2021/09/图片-59.png)

### 页面跳转

在首页wxml添加一个按钮,在JS文件绑定点击事件,跳转页面.

```
bindtap="bindViewMusic"  //绑定函数
```

页面跳转: **wx.navigateTo**

```js
    wx.navigateTo({
      url: '../music/music'
    })
```

## music界面

### 实时显示搜索结果

就是能够实现每输入一个字就能显示对应的结果,而不是必须全部输入完,点击搜索才进行搜索。那么搜索按钮也省去了没什么用，一个输入框就够了。

```html
<input class="search-input" maxlength="10" bindinput="bindKeyInput" placeholder="请输入你想要听的歌曲" />
```

**bindinput** ， 编辑器内容改变时触发 ，调用bindKeyInput函数，即每次输入都会触发。

```js
bindKeyInput:function(e) {        
      var that =this;
      wx.request({
        url: 'https://autumnfish.cn/search?keywords=' + e.detail.value,
        success(res) {
          that.setData({
             objectArray: res.data.result.songs //将查询到的数据实时渲染到列表中
          });
        }
      });
    }
```

[**wx.request**](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html) ，发起 HTTPS 网络请求。调用API接口。

调用成功后将查询到的数组数据存储到 objectArray ，然后通过wx:for，列表渲染到界面上。

```
<view id="{{item.id}}" bindtap="playerView" wx:for="{{objectArray}}" >{{index+1}}、{{item.name}}--{{item.artists[0].name}} </view>
```

在组件上使用 **wx:for** 控制属性绑定一个数组，即可使用数组中各项的数据重复渲染该组件。默认数组的当前项的下标变量名默认为 index，数组当前项的变量名默认为 item。

可先通过控制台将接受数据打印出来，查看数据形式。比如搜索“天下”，如下：

![](http://yujing.fit/wp-content/uploads/2021/09/图片-60.png)

## player界面

### 页面数据传递

下一步为渲染到页面的歌曲列表绑定事件，使其点击能被跳转到播放页面，播放歌曲。

每一首歌都不同，怎么确定播放的点击那首呢？那就需要点击跳转时，还需要将当前点击的歌曲唯一Id，传入到下一个界面。然后通过Id获取到歌曲的播放地址url和歌曲名、歌手名等相关信息。在页面播放歌曲，并渲染出歌曲的相关信息。

```js
//跳转播放页面,带当前歌曲id传入下个页面
    playerView(e){
      wx.setStorageSync('id', e.currentTarget.id)
      var id = wx.getStorageSync('id');
      wx.navigateTo({
        url: '../player/player?id='+id
      })
     }  
```

**wx.setStorageSync(string key, any data)**

### 播放歌曲

在player界面接收到Id后，调用API接收到歌曲的相关信息。

```js
onLoad: function (option) { 
    var that =this;
    wx.request({ 
      url: 'https://autumnfish.cn/song/url?id=' + option.id, //根据id获取歌曲
      success(res) {
        //console.log(option.id) //得到当前点击歌曲id
        //console.log(res.data.data); //获取当前歌曲信息
        console.log(res.data.data[0].url)
        that.setData({
          musicUrl: res.data.data[0].url     //获取歌曲播放地址  
        });
      }
    })
    ///////留白,插入下片代码

}
```

接口地址不一样,所以会调用两次。

```js
      wx.request({ 
      url: 'https://autumnfish.cn/search?keywords=' + option.id, //根据id获取歌曲
      success(res) {
        that.setData({
          imgSrc: res.data.result.songs[0].artists[0].img1v1Url,  //获取专辑封面
          author: res.data.result.songs[0].artists[0].name,  //获取歌手名
          musicName: res.data.result.songs[0].name   //获取歌曲名
        });
      }
    })
```

默认控件上的作者名字，如果 controls 属性值为 false 则设置 author 无效

主要是觉得这个要简单一点，那个不知道怎么用。也懒得去研究研究，这个也够用了。

```html
<audio poster="{{imgSrc}}" name="{{musicName}}" author="{{author}}" src="{{musicUrl}}" id="myAudio" controls loop bindplay="funplay" bindpause="funpause" binderror="funerror"></audio>
```

将API接收到的数据，通过**数据绑定**到音频组件上。

基本功能实现完成。代码还有很多冗杂，不过至少能跑，管它什么轮子。

### [点击查看效果演示视频](https://wwr.lanzoui.com/iMByUu7sk8d)

2021-9-19-10:48