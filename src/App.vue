<template>
  <div class="container">
    <global-header :user="currentUser"></global-header>
    <validate-form @form-submit="onFormSubmit">
      <div class="mb-3">
        <label class="form-label">邮箱地址</label>
        <validate-input
            :rules="emailRules"
            v-model="emailVal"
            ref="inputRef"
            placeholder="请输入邮箱地址"
            type="text">

        </validate-input>
      </div>
      <div class="mb-3">
        <label class="form-label">密码</label>
        <validate-input
            :rules="password"
            v-model="passwordVal"

            placeholder="请输入密码"
            type="password">
        </validate-input>
      </div>
      <template #submit>
        <span class="btn btn-danger">submit</span>
      </template>
    </validate-form>

  </div>
</template>
<script lang="ts">
import { defineComponent, reactive, ref } from 'vue'
import 'bootstrap/dist/css/bootstrap.min.css'
// import ColumnList, { ColumnProps } from "./components/ColumnList.vue";
import ValidateInput, {RulesProp} from "@/components/ValidateInput.vue";
import GlobalHeader, { UserProps} from "@/components/GlobalHeader.vue";
import ValidateForm from "@/components/ValidateForm.vue";

const currentUser: UserProps = {
  isLogin: true,
  name: '西索',
}
// const testData: ColumnProps[] = [
//   {
//     id: 1,
//     title: 'test1的专栏',
//     description: '这是test1的专栏，有一段非常有意思的简介，可以更新一下的',
//     avatar:'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5f3e41a8b7d9c60b68cdd1ec.jpg?x-oss-process=image/resize,m_pad,h_50,w_50'
//   },
//   {
//     id: 2,
//     title: 'test2的专栏',
//     description: '这是test2的专栏，有一段非常有意思的简介，可以更新一下的',
//     avatar:'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5f3e3a17c305b1070f455202.jpg?x-oss-process=image/resize,m_pad,h_50,w_50'
//   },
//   {
//     id: 3,
//     title: 'test2的专栏',
//     description: '这是test2的专栏，有一段非常有意思的简介，可以更新一下的',
//     avatar:'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5f3e3a17c305b1070f455202.jpg?x-oss-process=image/resize,m_pad,h_50,w_50'
//   },
//   {
//     id: 4,
//     title: 'test1的专栏',
//     description: '这是test1的专栏，有一段非常有意思的简介，可以更新一下的',
//     avatar:'http://vue-maker.oss-cn-hangzhou.aliyuncs.com/vue-marker/5f3e41a8b7d9c60b68cdd1ec.jpg?x-oss-process=image/resize,m_pad,h_50,w_50'
//   },
// ]
const emailReg = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
export default defineComponent({
  name: 'App',
  components: {
    // ColumnList,
    ValidateForm,
    ValidateInput,
    GlobalHeader
  },

  setup() {
    const inputRef = ref<any>()
    const emailVal = ref('123@test.com')
    const emailRules: RulesProp = [
      {type:'required', message: '电子邮件地址不能为空'},
      {type:'email', message: '请输入正确的电子邮件格式'},
    ]
    const passwordVal =ref('123')
    const emailRef = reactive({
      val: '',
      error: false,
      message: ''
    })
    const validateEmail = () => {
      if(emailRef.val.trim() === '') {
        emailRef.error = true
        emailRef.message = 'can not be empty！'
      } else if (!emailReg.test(emailRef.val)) {
        emailRef.error = true
        emailRef.message = 'should be valid email！'
      }else {
        emailRef.error = false
        emailRef.message = ''
      }
    }

    const onFormSubmit = (result: boolean) => {
      console.log(inputRef.value.validateInput())
    }
    return{
      // list:testData,
      currentUser,
      emailRef,
      emailRules,
      emailVal,
      onFormSubmit,
      validateEmail,
      inputRef
    }
  },

})
</script>
<style>

</style>
