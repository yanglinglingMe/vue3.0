<template>
  <div class="hello">
    {{msg }}
    <div @click="clickTo">组件通信</div>
  </div>
</template>

<script>
import { reactive, toRefs } from 'vue'
export default {
  name: 'HelloWorld',
  props: {
    msg: {
      type: String,
      default: ''
    }
  },
  /*
  * 实际上等价于 vue2.x 版本的 beforeCreate 和 Created 这两个生命周期
  * */
  setup (props, content) {
    console.log(props, content, 'props')
    const state = reactive({
      list: ['水果', '酸奶', '化妆品']
    })
    const clickTo = () => {
      this.$emit('clickPlan', state.list)
    }
    return {
      ...toRefs(state),
      clickTo
    }
  }
}
</script>

<style scoped lang="scss">

</style>
