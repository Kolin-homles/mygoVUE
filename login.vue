<template>
<div class="layout">
    <div class="login">
        <img src="../assets/mygo4.jpg" alt="" class="tx">
        <el-form
            ref="ruleFormRef"
            style="display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;"
            
            :model="ruleForm"
            :rules="rules"
            label-width="auto"
        >
            <el-form-item label="账号" prop="username">
            <el-input v-model="ruleForm.username" @change="tx"/>
            </el-form-item>
            <el-form-item label="密码" prop="password">
            <el-input v-model="ruleForm.password" />
            </el-form-item>
           
            <el-form-item>
            <el-button type="primary" @click="submitForm(ruleFormRef)">
                登录
            </el-button>
            <el-button @click="resetForm(ruleFormRef)">重置</el-button>
            <el-button @click="register">注册</el-button>
            </el-form-item>
        </el-form>

    </div>
      
</div>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'
import { useRouter } from 'vue-router'
import { ElMessage } from 'element-plus'

//账号发生变化
const tx =(res:any)=>{
    console.log(res)
    // 向后台发送请求
    // 通过账号查询  用户头像
}
import { ro } from 'element-plus/es/locale/index.mjs'
//路由遥控器
const router = useRouter()

interface RuleForm {
  username: string,
  password: string
}
const ruleFormRef = ref<FormInstance>()
const ruleForm = reactive<RuleForm>({
  username: '' ,
  password: ''
  })
  const rules = reactive<FormRules<RuleForm>>({
  username: [
    { required: true, message: '请输入账号', trigger: 'blur' },
  ],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur' },
  ]
  })

  const submitForm = async (formEl: FormInstance | undefined) => {
  if (!formEl) return
  await formEl.validate((valid, fields) => {
    if (valid) {
      // 登录逻辑,验证必填项填了吗
      
      // ========== 修改开始：登录成功后传递用户名 ==========
      // 使用路由跳转时传递用户名参数
      router.push({
        path: '/home/user',
        query: {
          username: ruleForm.username
        }
      })
      // ========== 修改结束 ==========
      
      // 同时将用户名保存到本地存储，防止刷新页面后丢失
      localStorage.setItem('username', ruleForm.username)
      
      ElMessage.success('登录成功！')
      console.log('submit!')
    } else {
      console.log('error submit!', fields)
    }
  })
}

const resetForm = (formEl: FormInstance | undefined) => {
  if (!formEl) return
  formEl.resetFields()
}

//注册
const register =()=>{
    //跳转注册页面
    router.push('/register')
}

</script>

<style scoped> 
.tx{
    height: 50px;
    width: 50px;
    border-radius: 50%;
    box-shadow: 5px 5px 5px rgba(255, 255, 255, 0.6);
}
.layout{
    height: 100%;
    width: 100%;
    display: flex;
    background: url('..\assets\mygo3.jpg') no-repeat;
    background-size: 100% 100%;
    align-items: center;
    justify-content: center;
}
.login{
    width: 300px;
    height: 300px;
    background: rgba(255,255,255,0.3);
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
}
</style>