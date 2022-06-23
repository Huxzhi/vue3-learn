> 版权声明：本文为博主「小满 zs」原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
> 原文地址 [blog.csdn.net](https://blog.csdn.net/qq1195566313/article/details/122773486)

# 模板 [插值](https://so.csdn.net/so/search?q=%E6%8F%92%E5%80%BC&spm=1001.2101.3001.7020) 语法

### 在 script 声明一个变量可以直接在 template 使用用法为{{ 变量名称 }}

```vue
<template>
  <div>{{ message }}</div>
</template>
 
<script setup lang="ts">
const message = "我是小满"
</script>
```

### 模板语法是可以编写条件运算的

```vue
<template>
  <div>{{ message == 0 ? '我是小满 0' : '我不是小满 other' }}</div>
</template>
 
<script setup lang="ts">
const message:number = 1
</script>
```

### 运算也是支持的

```vue
<template>
  <div>{{ message  + 1 }}</div>
</template>

<script setup lang="ts">
const message:number = 1
</script>
```

### 操作 API 也是支持的

```vue
<template>
  <div>{{ message.split('，') }}</div>
</template>
 
<script setup lang="ts">
const message:string = "我，是，小，满"
</script>
```

### 指令

v- 开头都是 vue 的指令

v-text 用来显示文本

v-html 用来展示富文本

v-if 用来控制元素的显示隐藏（切换真假 DOM）

v-else-if 表示  `v-if`  的“else if 块”。可以链式调用

v-else v-if 条件收尾语句

v-show 用来控制元素的显示隐藏（display none block Css 切换）

v-on 简写@ 用来给元素添加事件

v-bind 简写：用来绑定元素的属性 Attr

v-model 双向绑定

v-for 用来遍历元素

---

### v-on 修饰符 冒泡案例

```vue
<template>
  <div @click="parent">
    <div @click.stop="child">child</div>
  </div>
</template>
 
<script setup lang="ts">
const child = () => {
  console.log('child');
 
}
const parent = () => {
  console.log('parent');
}
 
</script>
```

### 阻止表单提交案例
`@click.prevent` 点击事件生效，但是`submit` 不生效
```vue
<template>
  <form action="/">
    <button @click.prevent="submit" type="submit">submit</button>
  </form>
</template>
 
<script setup lang="ts">
const submit = () => {
  console.log('child');
 
}
 
</script>
```

### v-bind 绑定 class 案例 1

```vue
<template>
  <div :class="[flag ? 'active' : 'other', 'h']">12323</div>
</template>
 
<script setup lang="ts">
const flag: boolean = false;
</script>
 
<style>
.active {
  color: red;
}
.other {
  color: blue;
}
.h {
  height: 300px;
  border: 1px solid #ccc;
}
</style>
```

### v-bind 绑定 class 案例 2

```vue
<template>
  <div :class="[flag ? 'active' : 'other', 'h']">12323</div>
</template>
 
<script setup lang="ts">
const flag: boolean = false;
</script>
 
<style>
.active {
  color: red;
}
.other {
  color: blue;
}
.h {
  height: 300px;
  border: 1px solid #ccc;
}
</style>
```
还可以进行条件控制
```vue
<template>
  <div :class="flag">{{flag}}</div>
</template>
 
<script setup lang="ts">
type Cls = {
  other: boolean,
  h: boolean
}
const flag: Cls = {
  other: false,
  h: true
};
</script>
 
<style>
.active {
  color: red;
}
.other {
  color: blue;
}
.h {
  height: 300px;
  border: 1px solid #ccc;
}
</style>
```

### v-bind 绑定 style 案例

```vue
<template>
  <div :style="style">2222</div>
</template>
 
<script setup lang="ts">
 
type Style = {
  height: string,
  color: string
}
 
const style: Style = {
  height: "300px",
  color: "blue"
}
 
</script>
```

### v-model 案例

一般用于绑定表单

`import { ref } from 'vue'    const message = ref("v-model")`  
属性变成响应式的，进行实时变化，原因是 vue 进行属性劫持，

```vue
<template>
  <input v-model="message" type="text" />
  <div>{{ message }}</div>
</template>
 
<script setup lang="ts">
import { ref } from 'vue'
const message = ref("v-model")
</script>
 
<style>
.active {
  color: red;
}
.other {
  color: blue;
}
.h {
  height: 300px;
  border: 1px solid #ccc;
}
</style>
```
