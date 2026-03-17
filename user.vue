<template>
<div class="layout">
    <div class="user-header">
        <div class="header-left">
            <el-input
                v-model="username"
                style="width: 210px"
                placeholder="请输入要查询的成员账号"
                clearable
             />
             <el-button type="primary">查询</el-button>
        </div>
        <div class="header-right">
            <el-button type="success" @click="openDialog">添加成员</el-button>
        </div>
    </div>

    <el-table :data="tableData" border style="width: 100%;height: 600px;">
    <el-table-column prop="id" label="id" />
    <el-table-column prop="username" label="名字"  />
    <el-table-column prop="gender" label="性别" >
        <template #default="scoped">
        {{ scoped.row.gender== '0' ? '女' : '男'}}
        </template>   
    </el-table-column>
    <el-table-column prop="role" label="角色" >
        <template #default="scoped">
            <el-tag v-if="scoped.row.role == '吉他手'" type="primary">mygo成员</el-tag>
            <el-tag v-else type="success">苦来兮苦成员</el-tag>
        </template>
    </el-table-column>

    <el-table-column prop="imagePath" label="头像" >
         <template #default="scoped">
            <el-image
                style="width: 100px; height: 100px"
                :src="scoped.row.imagePath"
                :zoom-rate="1.2"
                :max-scale="7"
                :min-scale="0.2"
                :preview-src-list="[scoped.row.imagePath]"
                show-progress
                :initial-index="4"
                fit="cover"
                />
    </template>
    </el-table-column>

    <el-table-column label="操作" >
        <template #default="scoped">
            <el-button type="warning" @click="editUser(scoped.row)">编辑</el-button>
            <el-button type="danger" @click="deleteUser(scoped.row)">删除</el-button>
        </template>
    </el-table-column>
    </el-table>

    <el-pagination
      v-model:current-page="queryPage.pageNum"
      v-model:page-size="queryPage.pageSize"
      :page-sizes="[10, 20, 30, 40]"
      layout="total, sizes, prev, pager, next, jumper"
      :total="400"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    />
     <el-dialog
        v-model="dialogVisible"
        title="添加"
        width="500"
    >
        <el-form  :model="form" label-width="auto" style="max-width:600px">
            <el-form-item label="账号">
                <el-input v-model="form.username"/> 
            </el-form-item>
            <el-form-item label="密码">  
                <el-input v-model="form.password"/> 
            </el-form-item>
            <el-form-item label="性别">   
                <el-radio-group v-model="form.gender">
                    <el-radio :value="0">女</el-radio>
                    <el-radio :value="1">男</el-radio>
                </el-radio-group>
            </el-form-item>
            <el-form-item label="权限">   
                <el-radio-group v-model="form.role">
                    <el-radio :value="1">mygo成员</el-radio>
                    <el-radio :value="2">苦来兮苦成员</el-radio>
                </el-radio-group>
            </el-form-item>

            <el-form-item label="头像"> 
             
                <el-upload
                    class="avatar-uploader"
                    action="https://run.mocky.io/v3/9d059bf9-4660-45f2-925d-ce80ad6c4d15"
                    :on-success="handleAvatarSuccess"
                >
                    <img v-if="imageUrl" :src="imageUrl" class="avatar" />
                    <el-icon v-else class="avatar-uploader-icon"><Plus /></el-icon>
                </el-upload>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm">
                    登录
                </el-button>
                <el-button @click="resetForm">重置</el-button>
            </el-form-item> 

        </el-form>
    </el-dialog>
    
</div>
</template>
<script lang="ts" setup>

import { ref } from 'vue'
import { ElMessage } from 'element-plus'
import { Plus } from '@element-plus/icons-vue'

import type { UploadProps } from 'element-plus'



const username = ref('')

const dialogVisible = ref(false);

const form = ref({
    username: '',
    password: '',
    gender: 0,
    role: 1,
    imagePath: ''
})

const queryPage = ref({
    username: '',
    pageSize: 10,
    pageNum: 1
})

const tableData = ref([
    {
        id: 1,username: '千早爱音',gender: '0',role: '吉他手',imagePath: 'https://th.bing.com/th/id/OIP.udxlK4EXqwLMi4Wv_xi3rwHaEK?w=316&h=180&c=7&r=0&o=7&dpr=1.3&pid=1.7&rm=3'
    },
    {   
        id: 2,username: '长崎素世',gender: '0',role: '贝斯手',imagePath: 'https://www.bing.com/th/id/OIP.cOVRU1XwPup5hNbb4BYd_QHaMS?w=160&h=211&c=8&rs=1&qlt=90&o=6&dpr=1.3&pid=3.1&rm=2'
    },
    {   
        id: 3,username: '高松灯',gender: '0',role: '主唱',imagePath: 'https://th.bing.com/th/id/OIP.3Nf2jJbCwLoxwzyi3yMbNgAAAA?w=202&h=210&c=7&r=0&o=7&dpr=1.3&pid=1.7&rm=3'
    },
    {   
        id: 4,username: '要乐奈',gender: '0',role: '吉他手',imagePath: 'https://th.bing.com/th/id/OIP.K0qlHdeSi757PtZ3yf_uvgHaEK?w=306&h=180&c=7&r=0&o=7&dpr=1.3&pid=1.7&rm=3'
    },
    {   
        id: 5,username: '椎名立希',gender: '0',role: '鼓手',imagePath: 'https://th.bing.com/th/id/OIP.sibi7TgrawI67AYMkJqtTgHaHa?w=209&h=209&c=7&r=0&o=7&dpr=1.3&pid=1.7&rm=3'
    }
])

const openDialog = () =>{
   dialogVisible.value = true
}

const editUser = (row:any) =>{
    console.log('编辑',row)
}

const deleteUser = (row:any) =>{
    console.log('删除',row)
}
const handleSizeChange = (val: number) => {
    queryPage.value.pageSize = val

    console.log(`${val} items per page`)
}
const handleCurrentChange = (val: number) => {
    queryPage.value.pageNum = val
    console.log(`current page: ${val}`)
}


const handleAvatarSuccess: UploadProps['onSuccess'] = (
  response,
  uploadFile
) => {
  form.value.imagePath = response.data.path;
}

const submitForm = ()=>{
    //提交表单
}
const resetForm = ()=>{
    //取消表单//关闭
}


</script>
<style scoped>
    .avatar-uploader .avatar {
    width: 178px;
    height: 178px;
    display: block;
    }
    .header-left{
        width: 300px;
        height: 100%;
        display: flex;
        justify-content: space-around;
        align-items: center;
    }
    .header-right{
        width: 300px;
        height: 100%;
        display: flex;
        justify-content: space-around;
        align-items: center;
    }
    .layout{
        width: 100%;
        height: 100%;
    }
    .user-header{
        height: 50px;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
</style>

<style>
.avatar-uploader .el-upload {
  border: 1px dashed var(--el-border-color);
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transition: var(--el-transition-duration-fast);
}

.avatar-uploader .el-upload:hover {
  border-color: var(--el-color-primary);
}

.el-icon.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  text-align: center;
}
</style>