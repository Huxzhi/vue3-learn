> 版权声明：本文为博主「小满 zs」原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
> 原文地址 [blog.csdn.net](https://blog.csdn.net/qq1195566313/article/details/122780637)

## ref

接受一个内部值并返回一个响应式且可变的 ref 对象。ref 对象仅有一个 `.value` property，指向该内部值。

案例

我们这样操作是无法改变 message  的值 应为 message 不是响应式的无法被 vue 跟踪要改成 ref

```vue
<template>
  <div>

    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>

  </div>
</template>
 
<script setup lang="ts">
let message: string = "我是 message"
 
const changeMsg = () => {
   message = "change msg"
}
</script>
```

改为 ref

Ref TS 对应的接口

```vue
interface Ref<T> {
  value: T
}
```

注意被 ref 包装之后需要。value 来进行赋值
在`<template> {{ message }}</template>` 会自动读取 value 值

```vue
<template>
  <div>

    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>

  </div>
</template>
 
<script setup lang="ts">
import {ref, Ref} from 'vue'
let message: Ref<string> = ref("我是 message")
 
const changeMsg = () => {
   message.value = "change msg"
}
</script>
 

//--------------------------------ts 两种方式
<template>
  <div>

    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>

  </div>
</template>
 
<script setup lang="ts">
import { ref } from 'vue'
let message = ref<string | number>("我是 message")
 
const changeMsg = () => {
  message.value = "change msg"
}
</script>
```

## `isRef`

判断是不是一个 ref 对象

```
<script setup lang="ts">
import { ref, Ref, isRef } from 'vue'
let message: Ref<string | number> = ref("我是 message")
let notRef:number = 123
const changeMsg = () => {
  message.value = "change msg"
  console.log(isRef(message)); //true
  console.log(isRef(notRef)); //false
  
}
</script>
```

## `shallowRef`

创建一个跟踪自身 `.value` 变化的 ref，但不会使其值也变成响应式的

例子

修改其属性是非响应式的这样是不会改变的

```vue
<template>
  <div>

    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>

  </div>
</template>
 
<script setup lang="ts">
import { Ref, shallowRef } from 'vue'
type Obj = {
  name: string
}
let message: Ref<Obj> = shallowRef({
  name: "小满"
})
 
const changeMsg = () => {
  message.value.name = '大满'
}
</script>
 
```

例子 2

这样是可以被监听到的修改 value

```vue
<script setup lang="ts">
import { Ref, shallowRef } from 'vue'
type Obj = {
  name: string
}
let message: Ref<Obj> = shallowRef({
  name: "小满"
})
 
const changeMsg = () => {
  message.value = { name: "大满" }
}
</script>
```

## `triggerRef` 

强制更新页面 DOM
这样也是可以改变值的

```vue
<template>
  <div>

    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>

  </div>
</template>
 
<script setup lang="ts">
import { Ref, shallowRef, triggerRef } from 'vue'
type Obj = {
  name: string
}
let message: Ref<Obj> = shallowRef({
  name: "小满"
})
 
const changeMsg = () => {
  message.value.name = '大满'
 triggerRef(message)
}
</script>
```

## customRef

自定义 ref 

customRef 是个工厂函数要求我们返回一个对象 并且实现 get 和 set

```vue
<script setup lang="ts">
import { Ref, shallowRef, triggerRef, customRef } from 'vue'
 
function Myref<T>(value: T) {
  return customRef((track, trigger) => {

    return {
      get() {
        track() //收集依赖
        return value
      },
      set(newVal: T) {
        console.log('set');
        value = newVal
        trigger() //触发依赖更新
      }
    }

  })
}
 
let message = Myref('小满')
const changeMsg = () => {
  message.value = '大满'
  // triggerRef(message)
}
</script>
```

> ps：如果同时引用了 `Ref` 和 `shallowRef` `，在同一个触发点击事件内，Ref` 会触发  `triggerRefValue` ，导致 `shallowRef` 也受到更新视图。[原理解释](https://www.bilibili.com/video/BV1dS4y1y7vd?p=11)