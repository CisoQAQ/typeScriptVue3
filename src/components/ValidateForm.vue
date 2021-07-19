<template>
  <form class="validate-form-container">
    <slot name="default"></slot>
    <div class="submit-area" @click.prevent="submitForm">
      <slot name="submit">
        <button type="submit"  class="btn btn-primary"> 提交 </button>
      </slot>

    </div>
  </form>
</template>

<script lang="ts">
import { defineComponent, onUnmounted  } from  'vue'
import mitt from 'mitt'
type ValidateFunc = () => boolean
type Events = {
  'form-item-created': ValidateFunc
}
export const emitter = mitt<Events>()
export default defineComponent({
  emits: ['form-submit'],
  setup(props, context) {
    const  submitForm = () => {
      context.emit('form-submit', true)
    }
    const callback = (test ?: string) => {
      console.log(test)
    }
    emitter.on('form-item-created', callback)
    onUnmounted(() => {
      emitter.off('form-item-created', callback)
    })
    return {
      submitForm,
      // callback
    }
  },
  mounted() {
    // this.$on('item-created', () => {
    //
    // })
  }
})
</script>
