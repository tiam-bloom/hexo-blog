---
title: Vue3项目基本搭建
cover: ''
tags:
  - Vite
  - Vue3
  - Vue-Router
  - Pinia
  - Element plus
  - TailwindCss
  - FontAwesome
categories:
  - Vue
abbrlink: 1725adc6
date: 2023-01-28 21:39:33
---

# 项目搭建

## vite

使用`vite`创建项目

```PowerShell
# 方式一
npm init/create vue [项目名]
# 方式二
npm init/create vite [项目名]
# 方式三
vue ui
```

安装 Vite 的前端依赖关系

```PowerShell
> npm i
```

## Vue-Router

Vue2 安装`npm i vue-router@3`

安装路由`Vue-Router`

```PowerShell
> npm i vue-router
// 当文件名为 index 时, 完整路径 ./router/index.js 可简写为 ./router
import router from './router'
app.use(router)
// router/index.js 基本使用
import { createRouter,createWebHashHistory } from "vue-router";

export default createRouter({
    history: createWebHashHistory(),
    routes: [
        {
            path: '/',
            redirect: { name: "Navigate" },
            // redirect: '/navigate',   // 通过路径重定向
        },
        {
            name: 'Navigate',
            path: '/navigate',
            component: () => import('@/views/Navigate.vue'),
            
        }
    ]
})
```

## Pinia

安装`Pinia`

```PowerShell
> npm i pinia
import { createPinia } from 'pinia'
const pinia = createPinia()
app.use(pinia)
// stores/siteInfo.js
import {defineStore} from 'pinia'

export const useSiteInfoStore = defineStore('siteInfo',{
    state: () => ({
        count: 0
    }),
    getters: {
        double: (state) => state.count * 2,
    },
    actions: {
        increment() {
            this.count++
        }
    }
})
```

使用

```JavaScript
// <script setup>
import { storeToRefs } from 'pinia'
import { useSiteStore } from '@/stores/siteInfo'
const siteStore = useSiteStore()
// 获取响应式的state
const { count, } = storeToRefs(siteStore)
// 获取action
const { increment } = siteStore
// 获取getters
...
```

## Element Plus

安装 `Element Plus`

```PowerShell
> $ npm i element-plus
// main.js 中
// 引入element-plus
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'  // 否则无样式, 但是不会报错
const app = createApp(App)
app.use(ElementPlus)
app.mount('#app')
```

安装`Element Plus`图标库

```PowerShell
> $ npm install @element-plus/icons-vue
// main.js 引入全部图标
import * as ElementPlusIconsVue from '@element-plus/icons-vue'
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}
```

按需引入图标(**推荐**)

```JavaScript
<template>
    <!-- 方式一 -->
    <el-input :prefix-icon="Search"/>
    <!-- 方式二 -->
    <el-icon style="vertical-align: middle">
      <Search />
    </el-icon>
    <!-- 方式三 -->
    <Search style="width: 1em; height: 1em; margin-right: 8px" />
</template>

<script setup>
import { Search } from '@element-plus/icons-vue'
</script>
```

## Tailwind CSS

安装Tailwind CSS:  https://www.tailwindcss.cn/docs/guides/vue-3-vite

```Bash
$ npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```

初始化配置文件, `tailwind.config.js` 和 `postcss.config.js` 文件：

```Bash
$ npx tailwindcss init -p
```

不管`postcss.config`中的配置

在`tailwind.config`中添加一行配置

```JavaScript
// tailwind.config.cjs
/** @type {import('tailwindcss').Config} */
module.exports = {
  purge: ['./index.html', './src/**/*.{vue,js,ts,jsx,tsx}'],  // 添加此行配置, 很重要!
  content: [],
  theme: {
    extend: {},
  },
  plugins: [
    require("tailwindcss"),
    require("autoprefixer"),
  ],
}
```

新建一个css文件, 并引入到`main.ts`中

```CSS
/* style.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

测试, 字体为粉色说明ok

```HTML
    <text class="text-pink-200">1111</text>
```

## Font Awesome图标

- CDN引入:    `<link rel="stylesheet" href="https://cdn.staticfile.org/font-awesome/4.7.0/css/font-awesome.css">`
- 用法:  https://www.runoob.com/font-awesome/fontawesome-reference.html



### VUE引入

- 安装:  https://fontawesome.com/v6/docs/web/use-with/vue/
- 用法Vue2:  https://blog.fontawesome.com/how-to-use-vue-js-with-font-awesome/
- 图标搜索:  https://fontawesome.com/icons

```shell
# add svg-core	
npm i --save @fortawesome/fontawesome-svg-core

# Free icons styles
npm i --save @fortawesome/free-solid-svg-icons
npm i --save @fortawesome/free-regular-svg-icons
npm i --save @fortawesome/free-brands-svg-icons

# for Vue 2.x
npm i --save @fortawesome/vue-fontawesome@latest-2

# for Vue 3.x
npm i --save @fortawesome/vue-fontawesome@latest-3
```

引入

```js
/* Set up using Vue 3 */
import { createApp } from 'vue'
import App from './App.vue'

/* import the fontawesome core */
import { library } from '@fortawesome/fontawesome-svg-core'
/* import font awesome icon component */
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
/* import specific icons */
import { faUserSecret } from '@fortawesome/free-solid-svg-icons'

/* add icons to the library */
library.add(faUserSecret)

createApp(App)
.component('font-awesome-icon', FontAwesomeIcon)
.mount('#app')
```

使用

```html
    <font-awesome-icon icon="fa-solid fa-user-secret" />
```

## VueUse

```shell
npm i @vueuse/core
```

### 暗黑模式`element plus` + `VueUse`

```js
// main.js中 引入样式
import 'element-plus/theme-chalk/dark/css-vars.css'
```

```html
<!-- html标签中添加 class="dark"  -->
<html lang="en" class="dark">  
```

使用

```vue
<template>
	<!-- El开关组件 -->
    <el-switch v-model="isDark" />
</template>

<script setup lang="ts">
import { useDark } from '@vueuse/core'
const isDark = useDark();
</script>
```



## 配置路径别名

```shell
npm i path
```

```Bash

// vite.config.js
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import path from "path";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
});
```
