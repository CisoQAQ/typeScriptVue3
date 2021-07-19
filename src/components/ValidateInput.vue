<template>
  <div class="validate-input-container pb-3"></div>
    <input
           class="form-control"
           id="exampleInputEmail1"
           :class="{'is-invalid': inputRef.error}"
           aria-describedby="emailHelp"
           :value="inputRef.val"
           @blur="validateInput"
           @input="updateValue"
           v-bind="$attrs"
    >
    <span v-if="inputRef.error" class="invalid-feedback">{{inputRef.message}}</span>
</template>

<script lang="ts">
import { defineComponent, reactive, PropType, onMounted } from  'vue'

import { emitter } from './ValidateForm.vue'
const emailReg = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
interface RuleProp {
  type: 'required' | 'email';
  message: string;
}
export type RulesProp = RuleProp[]
export default defineComponent({
  props: {
    rules: Array as PropType<RulesProp>,
    modelValue: String
  },
  inheritAttrs: false,
  name: "ValidateInput",
  setup(props, context){
    const inputRef = reactive({
      val: props.modelValue || '',
      error: false,
      message: ''
    })

    const updateValue = (e: KeyboardEvent) => {
      const targetValue = (e.target as HTMLInputElement).value
      inputRef.val = targetValue
      context.emit('update:modelValue', targetValue)
    }

    const validateInput = () => {
      if (props.rules) {
        const allPassed = props.rules.every(rule => {
          let passed = true
          inputRef.message = rule.message
          switch (rule.type) {
            case 'required':
              passed = (inputRef.val.trim() !== '')
              break;
            case 'email':
              passed = emailReg.test(inputRef.val)
              break;
            default:
              break;
          }
          return passed
        })
        inputRef.error = !allPassed
        return allPassed
      }
      return true
    }
    onMounted(() => {
      emitter.emit('form-item-created', inputRef.val)
    })
    return{
      inputRef,
      validateInput,
      updateValue
    }
  },

})
</script>
