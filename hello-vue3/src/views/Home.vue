<template>
  <div class="home">
    <HelloWorld :msg="msg" @clickPlan="clickPlan"/>
    <div>count:{{count}}<button @click="handlerCountAdd">Click ++</button>{{clickCount}}</div><br/>
    <!--反转字符串 demo-->
    <div>value:<input v-model="value"/><br/>rvalue:{{rvalue}}</div>
  </div>
</template>

<script>
/* eslint-disable */
import { reactive, toRefs,computed,watch } from 'vue'
import HelloWorld from '@/components/HelloWorld.vue'
export default {
  name: 'Home',
  components: {
    HelloWorld
  },
  data () {
    return {
      msg: '你好vue3.0'
    }
  },
  setup () {
    const state = reactive({
      count: 0,
      value: '',
      rvalue: computed(() => state.value.split('').reverse().join(''))
    })
    const handlerCountAdd = () => {
      state.count++
    }
    const clickPlan = (data) => {
      console.log(data,'data')
    }
    const clickCount = computed(() => {
      return state.count
    })
    watch(
      () => state.count,
      val => {
        console.log('watch',state.count)
      }
    )
    // effect(() => {
    //   console.log('effect',state.count)
    // })
    return {
      ...toRefs(state),
      handlerCountAdd,
      clickPlan,
      clickCount
    }
  }
}
</script>
