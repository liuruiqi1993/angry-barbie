# 通信

[https://juejin.im/post/5d267dcdf265da1b957081a3](https://juejin.im/post/5d267dcdf265da1b957081a3)

## props / $emit

## $children / $parent / $refs

## provide/inject

**provide和inject不是响应式的，这点很重要，除非申明的属性是响应式的！**

C是B的子组件，B是A的子组件

```text
// A.vue

<template>
  <div>
 <comB></comB>
  </div>
</template>

<script>
  import comB from '../components/test/comB.vue'
  export default {
    name: "A",
    provide: {
      for: "demo"
    },
    components:{
      comB
    }
  }
</script>
```

```text
// B.vue

<template>
  <div>
    {{demo}}
    <comC></comC>
  </div>
</template>

<script>
  import comC from '../components/test/comC.vue'
  export default {
    name: "B",
    inject: ['for'],
    data() {
      return {
        demo: this.for
      }
    },
    components: {
      comC
    }
  }
</script>
```

## eventBus

1 初始化

```javascript
// event-bus.js

import Vue from 'vue'
export const EventBus = new Vue()
```

```text
<template>
  <div>
    <show-num-com></show-num-com>
    <addition-num-com></addition-num-com>
  </div>
</template>

<script>
import showNumCom from './showNum.vue'
import additionNumCom from './additionNum.vue'
export default {
  components: { showNumCom, additionNumCom }
}
</script>
```

2 发送事件

```text
// addtionNum.vue 中发送事件

<template>
  <div>
    <button @click="additionHandle">+加法器</button>
  </div>
</template>

<script>
import {EventBus} from './event-bus.js'
console.log(EventBus)
export default {
  data(){
    return{
      num:1
    }
  },

  methods:{
    additionHandle(){
      EventBus.$emit('addition', {
        num:this.num++
      })
    }
  }
}
</script>
```

3 接收事件

```text
// showNum.vue 中接收事件

<template>
  <div>计算和: {{count}}</div>
</template>

<script>
import { EventBus } from './event-bus.js'
export default {
  data() {
    return {
      count: 0
    }
  },

  mounted() {
    EventBus.$on('addition', param => {
      this.count = this.count + param.num;
    })
  }
}
</script>
```

4 移除事件监听者

```javascript
import { eventBus } from 'event-bus.js'
EventBus.$off('addition', {})
```

## Vuex

## localStorage / sessionStorage

## $attrs 与 $listeners

$listeners本质是父组件上所有监听的事件，native除外。

