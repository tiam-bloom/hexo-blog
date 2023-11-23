---
title: vue3父子通信<script setup>语法
tags:
  - defineExpose
  - defineProps
  - defineEmits
  - vue
categories:
  - vue
abbrlink: f8ff8dc8
date: 2023-02-10 01:58:26
---

## 子传父

> 代码都不多,  很多注释,  仔细读代码

### defineExpose

> https://cn.vuejs.org/guide/essentials/template-refs.html#ref-on-component
>
> 如果一个子组件使用的是选项式 API 或没有使用 `<script setup>`，被引用的组件实例和该子组件的 `this` 完全一致，这意味着父组件对子组件的每一个属性和方法都有**完全的访问权**。这使得在父组件和子组件之间创建紧密耦合的实现细节变得很容易，当然也因此，应该只在绝对需要时才使用组件引用。**大多数情况下**，你应该首先**使用标准的 props 和 emit 接口**来实现**父子组件交互。**

将**子组件**中的数据传递给**父组件**中

> 父组件中**可直接修改**子组件暴露的值

```vue son.vue(子组件)
<template>
    <input type="text" v-model="data">
</template>
<!-- 使用了 <script setup> 的组件是默认私有的：
一个父组件无法访问到一个使用了 <script setup> 的子组件中的任何东西，
除非子组件在其中通过 defineExpose 宏显式暴露：-->
<script setup>
import { ref } from 'vue';
const data = ref('子组件中的值');
function fun() {
    console.log("子组件中的方法")
}
// 像 defineExpose 这样的编译器宏不需要导入
// https://cn.vuejs.org/api/sfc-script-setup.html#defineexpose
defineExpose({
    data,
    fun
})
</script>
```

```vue 父组件
// 父组件
<template>
	<!-- ref属性: https://cn.vuejs.org/api/built-in-special-attributes.html#ref -->
    子组件: <Son ref="sonData" /><br/>
    父组件: <span>{{ sonData }}</span>
</template>

<script setup>
import Son from './Son.vue'
import { ref, onMounted } from 'vue';
// 声明一个 ref 来存放该元素的引用
// 必须和模板里的 ref 同名
const sonData = ref();
 // !!!只能在挂载之后调用, 即创建DOM节点之后
onMounted(() => {
  // sonData.value 是子组件的实例
  // 调用子组件的方法
  sonData.value.fun();
  // 读取子组件的值
  console.log(sonData.value.data);
  // 修改子组件的值
  sonData.value.data = '修改后的值';
  console.log(sonData.value.data);
})
</script>
```

运行结果

<img src="http://qiniu.yujing.fit/typora_img/sonTOFather (1).gif">

> think: `setup`是最先运行的，在渲染DOM节点之前，`sonData`都是空的`ref()`，无法读取子组件中的值，直到渲染完`<Son ref="sonData" />`，即组件挂载完成后，才可访问。[生命周期↗](https://cn.vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)
>
> 注意: 不能直接写在`<script setup>`中，可封装在一个函数中，需要时再调用（调用时应在挂载完成后）
>
> [官网](https://cn.vuejs.org/api/built-in-special-attributes.html#ref)原句: "关于 ref 注册时机的重要说明：因为 ref 本身是作为渲染函数的结果来创建的，**必须等待组件挂载后才能对它进行访问**。"
>
> [组件上的ref↗](https://cn.vuejs.org/guide/essentials/template-refs.html#ref-on-component)

### defineEmits

- 父组件给子组件传递一个函数
- 子组件向函数传递参数，间接传递给父组件
- 父组件中**无法直接更改**子组件中的值

```vue 子组件
<template>
    <input type="text" v-model="data" />
</template>

<script setup>
import { ref } from 'vue';
const data = ref('子组件中的值');
const emits = defineEmits(['change']);
// 初始化时，触发一次change事件
emits('change', data.value);
// 监听data的变化，自动触发change事件, 将最新值传递给父组件
watch(data, newVal => {
    emits('change', newVal);
});    
</script>
```

```vue 父组件
<template>
  子组件: <Son @change="change"/><br />
  父组件: <span>{{ data }}</span><br />
</template>

<script setup>
import Son from './Son.vue'
import { ref } from 'vue';
 // 不应在父组件中修改data的值, 而是在子组件中调用
 // 修改此处的data并不会引起子组件中的值变化, 是子=>父单向的
const data = ref('');
function change(params) {
  data.value = params;
}
</script>
```



## 父传子

### defineEmits

使用[`defineEmits()`](https://cn.vuejs.org/api/sfc-script-setup.html#defineprops-defineemits)在子组件中声明所传递的事件

```vue
// 子组件
<template>
	<!-- 模板中可直接通过$emit调用事件 -->
    <button @click="$emit('change','参数')">button</button>
</template>

<script setup>
const emit = defineEmits(['change', 'delete'])
// 调用父组件中的change_函数
emit('change','参数');
// 调用父组件中的delete_函数
emit('delete','参数a','参数b');
</script>
```

```vue
// 父组件
<template>
	<!-- @符号之后的是子组件中调用的事件名, 通常名字保持一致或者类似 -->
    <Son @change="change_" @delete="delete_" /><br />
</template>

<script setup>
import Son from './Son.vue'
function change_(data) {
  console.log(data);
}
function delete_(a, b) {
  console.log(a, b);
}
</script>
```

#### 子组件中修改父组件的值

- 子组件中即可修改父组件键中的值
- 当然 父组件中 肯定也可以修改自己的值

```vue
// 子组件
<template>
    <div>
        <button @click="$emit('setName', '李四')">改为李四</button>
        <p>父组件的name: {{ name }}</p>
    </div>
</template>

<script setup>
const emit = defineEmits(['setName']);
const prop = defineProps(['name']);
emit('setName', '王五');
</script>
```

```vue
// 父组件
<template>
  <Son :name="name" @setName = setName />
</template>

<script setup>
import Son from './Son.vue'
import { ref } from 'vue'
let name  = ref('张三');
function setName(val) {
  name.value = val;
}
</script>
```

### defineProps

使用[`defineProps()`](https://cn.vuejs.org/api/sfc-script-setup.html#defineprops-defineemits)在子组件中声明所传递的属性

>  props是**单向数据流**，只在父组件中改变，传递给子组件，在子组件**只读**
>
> 想改变怎么办？[通过父组件向子组件传递set事件即可](#子组件中修改父组件的值), 通过父组件传递的函数调用间接改变父组件的值

```vue
// 子组件
<template>
    <div>
        <!-- 在模板中可直接使用, 无需 props.name -->
        <div>{{ name }}</div>
    	<div>{{ age }}</div>
    </div>
</template>

<script setup>
// 未使用defineprops声明的属性将会穿透到子组件的根元素上
// (如name与age未被声明, 将渲染为 => <div name="张三" age="18"></div>) 
// 没有根元素将不会渲染任何元素, 可通过绑定v-bind="$attrs" 属性决定渲染到哪个元素, 可绑定多个(禁用继承也生效)
const props = defineProps(['name', 'age']);  
// 读取
console.log(props.name, props.age);
// props.name = '李四';  // 只读的 Set operation on key "name" failed: target is readonly
```

```vue
// 父组件
<template>
    <Son name="张三" :age="age" /><br />
</template>

<script setup>
import Son from './Son.vue'
const age = 18;
</script>
```

- [属性穿透](https://cn.vuejs.org/guide/components/attrs.html#attribute-inheritance): “透传 attribute”指的是传递给一个组件，却没有被该组件声明为 [props](https://cn.vuejs.org/guide/components/props.html) 或 [emits](https://cn.vuejs.org/guide/components/events.html#defining-custom-events) 的 attribute 或者 `v-on` 事件监听器。最常见的例子就是 `class`、`style` 和 `id`。满足条件可深层传递

```vue
// 子组件
<template>
    <!-- 被age属性 defineProps"消费", 将不会出现在$attrs中 -->
    <div>{{ $attrs }} | {{age}}</div>  
    <!-- 渲染结果: <div name="张三">{"name": "张三"} | 18</div> -->
</template>

<script>
// 使用普通的 <script> 来声明选项, 禁用继承属性
// 禁用后将不会有name属性, 渲染结果 => <div>{"name": "张三"} | 18</div>
// ! 查看渲染结果请解开下面的注释
/* export default {
  inheritAttrs: false
} */
</script>

<script setup>
// 在 <script setup> 中访问 attrs  
import { useAttrs } from 'vue'
const attrs = useAttrs();  // 只读 =>  Proxy {name: '张三', __vInternal: 1}
    
// defineProps对象写法
defineProps({
    age: {
        type: Number,
        default: 20
    },
})
</script>
```

```vue
// 父组件
<template>
    <Son name="张三" :age="age" /><br />
</template>

<script setup>
import Son from './Son.vue'
const age = 18;
</script>
```

props**传递对象**的简写

```vue
// 子组件
<template>
    <!-- 渲染结果: <div name="张三" age="18">{ "name": "张三", "age": 18 }</div> -->
    <div>{{ $attrs }}</div>
</template>
```

```vue
// 父组件
<template>
  <Son v-bind="user" />
  <!-- 等同于 -->
  <Son :name="user.name" :age="user.age" />
</template>

<script setup>
import Son from './Son.vue'
const user = {
  name: '张三',
  age: 18
}
</script>
```

## 父子双向

[vue3模板渲染流程](https://kaifa.baidu.com/searchPage?wd=vue3%E6%A8%A1%E6%9D%BF%E6%B8%B2%E6%9F%93%E6%B5%81%E7%A8%8B&module=SEARCH)

### defiineExpose

- 双向绑定，无论在子组件中还是在父组件中都可以修改，并在另一个组件中得到响应
- 方式一： 通过defiineExpose，起点是子（数据源）暴露给父

```vue
// 子组件
<template>
    <input type="text" v-model="data">
</template>

<script setup>
import { ref } from 'vue';
const data = ref('子组件中的值');  // 数据源在子中

defineExpose({
    data,
})
</script>
```

```vue
// 父组件
<template>
  <!--只是随便加一个属性:q="sonData", 主要是读取sonData的值就能变正常, why?-->
  子组件: <Son ref="sonData" :q="sonData"/><br />
  父组件: <span>{{ data }}</span><br />
  父组件: <input type="text" :value="data" @input="setData"><br />
</template>

<script setup>
import Son from './Son.vue'
import { ref, onMounted, computed } from 'vue';
const sonData = ref();
let data;  // 只读的计算属性
onMounted(() => {
  // 通过计算属性响应式的获取子组件的值
  data = computed(() => sonData.value.data);
})
function setData(e) {
  sonData.value.data = e.target.value;
}
</script>
```

<img src="http://qiniu.yujing.fit/typora_img/sonTOFather (2).gif">

### Emit和Prop

- 方式二： 通过emit和props(推荐)， 起点是父（数据源）

```vue 子组件
// 写法一
<template>
    <div>
        <p>子组件: {{ name }}</p>
        <input type="text" :value="name" @input="$emit('setName', $event.target.value)">
    </div>
</template>

<script setup>
defineProps(['name']);  //声明name
</script>
```

```vue
// 写法二: 子组件还可以只使用模板中的函数, 不再需要script标签中的内容
<template>
    <div>
        <p>子组件: {{ $attrs.name }}</p>
        <input type="text" :value="$attrs.name" @input="$emit('setName', $event.target.value)">
    </div>
</template>
```

```vue 父组件
<template>
  <Son :name="name" @setName = setName />
  <div>父组件: {{ name }}</div>
  <input type="text" v-model="name">
</template>

<script setup>
import Son from './Son.vue'
import { ref } from 'vue'
let name  = ref('张三');  // // 数据源在父中
function setName(val) {
  name.value = val;
}
</script>
```

<img src="http://qiniu.yujing.fit/typora_img/sonTOFather (3).gif" />

## 总结

- **父传子**并在子中**只读**，用**props**
- 父传子，但在子中可写，需再传递一个改值的事件到子组件中，props+emits
- 子传父，在父中只读，用emit向父组件中传递参数的形式
- 子传父，在父中可写，用defineExpose

