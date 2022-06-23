> 版权声明：本文为博主「小满 zs」原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
> 原文地址 [blog.csdn.net](https://blog.csdn.net/qq1195566313/article/details/122791665)

# `toRef toRefs toRaw`

## `toRef`

如果原始对象是非响应式的就不会更新视图 数据是会变的

```vue
<template>
   <div>
      <button @click="change">按钮</button>
      {{state}}
   </div>
</template>
 
<script setup lang="ts">
import { reactive, toRef } from 'vue'
 
const obj = {
   foo: 1,
   bar: 1
}
 
const state = toRef(obj, 'bar')
// bar 转化为响应式对象
 
const change = () => {
   state.value++
   console.log(obj, state);
 
}
</script>
```

如果原始对象是响应式的是会更新视图并且改变数据的

`toref()` 可以用来代理 `reactive` 深层次的对象属性拿出来到外部使用更改变量值

## `toRefs`

可以帮我们批量创建 `ref` 对象主要是方便我们解构使用

`ref()`为深拷贝，`toRef/toRefs`为浅拷贝，作用是通过解构简化 DOM 层使用对象中属性的方式

当响应式对象的属性比较多的时候就需要解构简化代码，但是解构会失去响应式。使用 toRefs 解构响应式对象，解构的属性仍具有响应式。

```js
import { reactive, toRefs } from 'vue'
const obj = reactive({
   foo: 1,
   bar: 1
})
 
let { foo, bar } = toRefs(obj)
console.log(foo, bar);
const change =() => {
   foo.value++
   bar.value++
}
```
解构出来的元素也是响应式

## `toRaw`

将响应式对象转化为普通对象

```js
import { reactive, toRaw } from 'vue'
 
const obj = reactive({
   foo: 1,
   bar: 1
})
 
const state = toRaw(obj)
// 响应式对象转化为普通对象
 
const change = () => {
 
   console.log(obj, state);
 
}
```
直接调用静态资源，减少内存消耗