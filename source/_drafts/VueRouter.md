# VueRouter

> vite+vue3+vuerouter4 基本使用
>
> 嵌套路由 以及 导航方式 

```ts
// router/index.ts 创建router实例
import { createRouter, createWebHashHistory } from 'vue-router'
import Layout from '../layout/Index.vue'
// 导出router实例
export default createRouter({
    history: createWebHashHistory(),
    routes: [
        {
            path: '/',
            redirect: { name: 'Index' }
        },
        {
            // 使用 ? 修饰可选参数 id
            path: '/index/:id?',
            name: 'Index',
            component: Layout,
            redirect: { name: 'JavaScript' },
            children: [
                {
                    path: '',  // 路径不变化, 默认显示的首页
                    name: 'JavaScript',
                    component: () => import('../components/JavaScript.vue')
                },
                {
                    path: 'java/:version',
                    name: 'Java',
                    component: () => import('../components/Java.vue')
                },
                {
                    path: 'vue',
                    name: 'Vue',
                    component: () => import('../components/Vue.vue')
                }
            ]
        },

    ]
})
```

```ts
// main.ts 引入使用
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import router from './router'

const app = createApp(App);
app.use(router);
app.mount('#app');
```

结构

```vue
<!-- App.vue -->
<template>
    <router-view />
</template>
```

```vue
<!-- 布局界面 loyout/Index.vue -->
<template>
    <Aside />
    <Main />
</template>

<script setup lang='ts'>
import Aside from './Aside.vue'
import Main from './Main.vue'
</script>
```

```vue
<!-- 主体内容 layout/Main.vue -->
<template>
  <!-- https://router.vuejs.org/zh/guide/migration/#router-view-%E3%80%81-keep-alive-%E5%92%8C-transition -->
  <router-view v-slot="{ Component }">
    <transition name="fade-transform" mode="out-in">
      <keep-alive>
        <component :is="Component" />
      </keep-alive>
    </transition>
  </router-view>
</template>
```

```vue
<!-- 侧边导航栏 -->
<template>
  <div>
    <!-- 声明式导航, 通过自定义name -->
    <router-link :to="{ name: 'JavaScript', params: { id: 10086 } }">JavaScript</router-link>
    <br>
    <!-- 声明式导航, 通过路径 -->
    <router-link to="/index/vue">Vue</router-link>
    <br>
    <!-- 将19作参数, 传递到显示层 view -->
    <router-link to="/index/java/19">Java19</router-link>
    <br>
    <!-- 编程式导航 -->
    <button @click="jump1">Java8</button>
    <button @click="$router.push({ path: '/index/java/11' })">Java11</button>
  </div>
</template>
<script setup lang='ts'>
import { useRouter } from 'vue-router'
const router = useRouter();
function jump1() {
  router.push({
    name: "Java",
    params: {
      version: 8
    }
  })
}

</script>
```

组件Main内容

```vue
<!-- Java.vue -->
<template>
  <div>
    Java => code once, run anywhere <br>
    Java版本: {{ $route.params.version }}
  </div>
</template>
```

```vue
<!-- JavaScript.vue -->
<template>
  <div>
    渐进式JavaScript框架 {{ $route.params.id }}
  </div>
</template>
```

```vue
<!-- Vue.vue -->
<template>
  <div>
    Vue => 渐进式JavaScript框架
  </div>
</template>
```

