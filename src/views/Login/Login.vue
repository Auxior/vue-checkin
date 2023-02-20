<template>
  <div class="login">
    <div class="header">
      <span class="header-logo">
        <i class="iconfont icon-vue"></i>
        <i class="iconfont icon-icon-test"></i>
        <i class="iconfont icon-typescript"></i>
      </span>
      <div class="header-title">在线考勤系统</div>
    </div>
    <el-form ref="ruleFormRef" :model="ruleForm" :rules="rules" label-width="120px" class="main">
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="ruleForm.email" type="text" placeholder="请输入邮箱" />
      </el-form-item>
      <el-form-item label="密码" prop="pass">
        <el-input v-model="ruleForm.pass" type="password" placeholder="请输入密码" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm(ruleFormRef)" auto-insert-space>登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'

// import { useStore } from '@/store';
// const store = useStore()

interface User {
  email: string
  pass: string
}

const ruleFormRef = ref<FormInstance>()

const ruleForm = reactive<User>({
  email: '',
  pass: ''
})

const rules = reactive<FormRules>({
})

const submitForm = (formEl: FormInstance | undefined) => {
  if (!formEl) return
  formEl.validate((valid) => {
    if (valid) {
      console.log('submit!')
    } else {
      console.log('error submit!')
      return false
    }
  })
}
</script>

<style scoped lang="scss">
.login {
  width: 100vw;
  height: 100vh;
  background: url('@/assets/images/login-bg.svg') no-repeat center 110px;
  background-size: 100%;

  .header {
    height: 44px;
    line-height: 44px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 34px;
    padding-top: 100px;

    .header-logo {

      .icon-vue,
      .icon-icon-test,
      .icon-typescript {
        margin-right: 5px;
        font-size: inherit;
      }

      .icon-vue {
        color: green;
      }

      .icon-icon-test {
        color: #deb887;
      }

      .icon-typescript {
        color: blue;
      }
    }

    .header-title {
      margin-left: 30px;
      font-weight: 700;
      font-size: 30px;
    }
  }

  .main {
    width: 500px;
    margin: 0 auto;
    padding-top: 50px;
  }
}
</style>