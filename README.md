# Vue
Vue 学习

# 经典问题
## 选项式 API 中的 inheritAttrs
### 情景
父组件 App.vue
```
<script setup>
import firstButton from './First.vue'
const handleClick = () => {
  console.log('点击按钮');
}
</script>

<template>
  <first-button name="111" @click="handleClick"></first-button>
</template>
```
子组件 First.vue
```
<script setup>
import secondButton from './Second.vue'
</script>

<template>
  <second-button></second-button>
</template>
```
后代组件 Second.vue
```
<template>
  <button>我是一个button</button>
</template>
```
### Q
1. App.vue 为自定义的组件 first-button 绑定了 click 事件，浏览器中可以打印出日志信息吗，为什么会发生这种事情？
2. 怎样可以使浏览器不打印出日志信息？
### A
1. 可以打印出日志信息。为 first-button 组件绑定的 name 属性和 click 事件透传到子组件 First.vue，进一步透传到后代组件 Second.vue 的原生 button 元素上了。
![image](https://user-images.githubusercontent.com/42236890/200836422-c0239c38-aac1-4cbe-bf48-d46d5e8894ba.png)
2. **默认情况下，父组件传递的，但没有被子组件解析为 props 的 attributes 绑定会被“透传”。** 这意味着当我们**有一个单根节点的子组件**时，这些绑定会被作为一个常规的 HTML attribute 应用在子组件的根节点元素上。使浏览器不打印出日志信息有两种方法：
  - 方法一（适用于选项式 API）：我们可以为子组件或后代组件设置 inheritAttrs 为 false 来禁用这个默认行为（inheritAttrs 用于控制是否启用默认的组件 attribute 透传行为）。
  - 方法二（适用于选项式和组合式 API）：将子组件和后代组件改写成非单根节点。
```
// 示例1：修改 First.vue
<script setup>
import secondButton from './Second.vue'
</script>

<template>
  <div>
    <second-button></second-button>
  </div>
  <div>
  </div>
</template>
// 示例2：修改 Second.vue（有两个 button 时不知道透传给哪个 button，做了都不透传的处理）
<template>
  <button>我是一个button</button>
  <button>我是另一个button</button>
</template>
```
### 参考
https://juejin.cn/post/6917819182994341895
