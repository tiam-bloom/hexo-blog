---
title: 目录锚点Demo
tags:
  - js
categories:
  - JavaScript
abbrlink: e986e4b8
date: 2023-02-19 22:14:53
---



## 目录锚点Demo

> 修改自[参考文章1](https://blog.csdn.net/qq_43684588/article/details/125048254)，添加一些注释，精简了一些内容

### HTML

```html
    <div class="container">
        <!-- 内容区域 -->
      <div class="content">
        <div data-tab="districtName1" class="districtName">
          <h2 class="data-tab">页面一</h2>
          <div>
            <ul>
                <li>当标题滚动到顶部时, 切换到当前目录</li>
            </ul>
          </div>
        </div>
        <div data-tab="districtName2" class="districtName">
          <h2 class="data-tab">页面二</h2>
          <div>页面二内容...</div>
        </div>
        <div data-tab="districtName3" class="districtName">
          <h2 class="data-tab">页面三</h2>
          <div>页面三内容...</div>
        </div>
        <div data-tab="districtName4" class="districtName">
          <h2 class="data-tab">页面四</h2>
          <div>页面四内容...</div>
        </div>
        <div data-tab="districtName5" class="districtName">
          <h2 class="data-tab">页面五</h2>
          <div>页面五内容...</div>
        </div>
      </div>
      <!-- 目录容器 -->
      <div class="directory">
        <div id="category">
          <ul></ul>
        </div>
      </div>
    </div>
    <!-- 底部填充物, 避免无法滑到最后一个目录 -->
    <div style="height: 500px; background: #eee"></div>
```

### CSS

```css
	  html,
      body {
        margin: 0;
        padding: 0;
        height: 100%;
      }

      .container {
        width: 100%;
        display: flex;
        padding: 20px 50px;
        box-sizing: border-box;
      }

      .directory {
        width: 200px;
      }

      .content {
        flex: 1;   
        display: flex;
        flex-direction: column;
      }

      .content h2 {
        margin: 0;
      }

      .districtName {
        width: 100%;
      }
      .districtName div {
        height: 500px;
        border: 1px solid #eee;
      }

      .container .directory ul,
      .container .directory li {
        list-style: none;
      }

      #category {
        /*粘性定位*/
        position: sticky;
        top: 16px;
        right: 0;
      }

      #category a {
        padding: 0 16px;
        color: #2d2e2f;
        outline: none;
        text-decoration: none;
        line-height: 30px;
      }

      #category a:hover {
        background: #efefef;
        cursor: pointer;
      }

      #category a.active {
        background: #efefef;
        color: #1890ff;
      }
```

### JavaScript

- 根据类名`class="data-tab"`生成目录,
- 为每个目录添加点击事件，点击滚动到目标内容，（根据`data-tab`的属性值，目录与内容的`data-tab`属性一一对应，类似id吧)
- 添加全局滚动监听事件, 触发时遍历内容, 判断哪个内容元素到顶部的距离小于100, 并激活内容元素对应的目录
- 目录激活通过添加类名`class=active`实现
- 滚动监听需获取两个值`scrollTop`和`offsetTop `
- scrollTop指当前可视窗口滚动的距离，向下滚动逐渐增大
- offsetTop指当前元素相对于其 [`offsetParent`](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FHTMLElement%2FoffsetParent) 元素的顶部内边距的距离，这里是`class="districtName"`的`div`容器距离`class="container"`的div容器的内边距，所以其每个元素的`offsetTop`是一个定值,并不随滚动而改变
- [参考文章2](https://juejin.cn/post/7037389069407485959)中有画图解释，或者MDN中[offsetTop](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetTop) ， [scrollTop](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/scrollTop)
- [scrollIntoView](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/scrollIntoView)滚动**目标元素**的**父元素**，使目标元素可见。

```js
    class dataScroll {
      // 获取内容节点数组
      districtName = document.querySelectorAll(".container .districtName");
      // 目录容器
      directoryDom = document.querySelector(".directory #category ul");
      constructor() {
        this.initDom(); //初始化目录dom
        this.initScroll(); //初始化监听滚动
      }
      initDom() {
        let Listdom = "";
        // Element.innerHTML 属性设置或获取 HTML 语法表示的元素的后代。
        this.districtName.forEach((item) => {
          const linkClass = item.querySelector(".data-tab");
          Listdom += `<li><a data-tab=${item.getAttribute("data-tab")}>${linkClass.innerHTML}</a></li>`;
        });
        this.directoryDom.innerHTML = Listdom;
        // 给所有目录节点添加点击事件
        const Alist = this.directoryDom.querySelectorAll("li a");
        Alist.forEach((el) => {
          el.addEventListener("click", (e) => {
            // 遍历所有内容节点
            this.districtName.forEach((targetNode) => {
              if (targetNode.getAttribute("data-tab") === e.target.getAttribute("data-tab")) {
                //页面直接滚动不过度 A标签高亮
                // document.documentElement.scrollTop = targetNode.offsetTop

                // 页面动画滚动过度 A标签高亮
                // Element 接口的 scrollIntoView() 方法会滚动元素的父容器，使被调用 scrollIntoView() 的元素对用户可见。
                // https://developer.mozilla.org/zh-CN/docs/Web/API/Element/scrollIntoView
                targetNode.scrollIntoView({
                  behavior: "smooth",
                  block: "start",
                  inline: "start",
                });
              }
            });
          });
        });
      }
      initScroll() {
        // 添加滚动监听
        window.addEventListener("scroll", (e) => {
          // Document.documentElement 是一个会返回文档对象（document）的根元素的只读属性（如 HTML 文档的 <html> 元素）。
          // Document.body 表示当前文档中的 <body> 或 <frameset> 元素，或 null 如果不存在此类元素。
          // Element.scrollTop 属性可以获取或设置一个元素的内容垂直滚动的像素数。
          const scroll = document.documentElement.scrollTop || document.body.scrollTop;
          this.districtName.forEach((item) => {
            // parseInt 解析一个字符串并返回指定基数的十进制整数
            // ceil 向上取整
            // HTMLElement.offsetTop 返回当前元素相对于其 offsetParent 元素的顶部内边距的距离
            if (item.offsetTop - scroll < 100) {  // 标题距离顶部小于100时, 切换当前激活的目录
              this.activeLoadScroll(item.getAttribute("data-tab"));
              return;
            }
          });
        });
      }
      /**
       * 遍历目录节点, 高亮当前滚动到的节点
       * @param {string} sections 属性data-tab的值
       */
      activeLoadScroll(sections) {
        const categoryDomA = document.querySelectorAll(".directory #category ul li a");
        categoryDomA.forEach((item) => {
          // 添加激活类名active
          item.className = item.getAttribute("data-tab") == sections ? "active" : "";
        });
      }
    }
```

### 优化

- 滚动监听添加防抖

```js
            window.addEventListener("scroll", debounce((e) => {
                const scroll = document.documentElement.scrollTop || document.body.scrollTop;
                this.districtName.forEach((item) => {
                    if (item.offsetTop - scroll < 100) {
                        this.activeLoadScroll(item.getAttribute("data-tab"));
                        return;
                    }
                });
            }));
            function debounce(fun, delay = 500) {
                let timer = null;
                return function (...args) {
                    clearTimeout(timer);
                    timer = setTimeout(() => {
                        fun.apply(this, args)
                    }, delay)
                }
            }
```



### 效果

![anchor](http://qiniu.yujing.fit/typora_img/anchor.gif)

### 相关链接

- 参考文章1: https://blog.csdn.net/qq_43684588/article/details/125048254
- 参考文章2: https://juejin.cn/post/7037389069407485959
