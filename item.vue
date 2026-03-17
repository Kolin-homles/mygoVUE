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
            <el-button type="success">添加成员</el-button>
        </div>
    </div>

    <el-table :data="tableData" border style="width: 100%;height: 600px;">
    <el-table-column prop="id" label="id" />
    <el-table-column prop="username" label="名字"  />
    <el-table-column prop="gender" label="专辑" >
        <template #default="scoped">
        {{ scoped.row.gender== '0' ? 'mygo' : 'Ave Mujica'}}
        </template>   
    </el-table-column>
    <el-table-column prop="role" label="乐队" >
        <template #default="scoped">
            <el-tag v-if="scoped.row.role == '1'" type="primary">MyGo!!!!!</el-tag>
            <el-tag v-else type="success">Ave Mujica</el-tag>
        </template>
    </el-table-column>

    <el-table-column prop="imagePath" label="封面" >
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
</div>
</template>
<script lang="ts" setup>
import { el, te } from 'element-plus/es/locale/index.mjs';
import { ref } from 'vue'
import { EditableTreeNode } from 'vue-router/dist/unplugin/index.cjs';
const username = ref('')

const queryPage = ref({
    username: '',
    pageSize: 10,
    pageNum: 1
})

const tableData = ref([
    {
        id: 1,username: '春日影',gender: '0',role: '1',imagePath: 'https://ts3.tc.mm.bing.net/th/id/OIP-C.LfU1Z7aH83ikrHShpDtfgwAAAA?rs=1&pid=ImgDetMain&o=7&rm=3'
    },
    {   
        id: 2,username: '影色舞',gender: '0',role: '1',imagePath: 'https://ts4.tc.mm.bing.net/th/id/OIP-C.mcmZdjb5Ai7BsvzzoqjoWAHaHa?rs=1&pid=ImgDetMain&o=7&rm=3'
    },
    {   
        id: 3,username: '迷星叫',gender: '0',role: '1',imagePath: 'https://tse3-mm.cn.bing.net/th/id/OIP-C.49fqKh-iyZpw-Kfo1hpCNgHaHT?w=172&h=180&c=7&r=0&o=7&dpr=1.3&pid=1.7&rm=3'
    },
    {   
        id: 4,username: '猛独侵袭',gender: '0',role: '1',imagePath: 'https://ts4.tc.mm.bing.net/th/id/OIP-C.HITMKAC9FFntbKhgzQHDHwHaEo?w=108&h=108&c=1&bgcl=cb38c2&r=0&o=7&dpr=1.3&pid=ImgRC&rm=3'
    },
    {   
        id: 5,username: '诗超绊',gender: '0',role: '1',imagePath: 'https://tse1-mm.cn.bing.net/th/id/OIP-C.w4XAnb-BIm2ICZcqPLOdGgHaHa?w=164&h=180&c=7&r=0&o=7&dpr=1.3&pid=1.7&rm=3'
    },
    {   
        id: 6,username: '颜',gender: '1',role: '0',imagePath: 'https://ts4.tc.mm.bing.net/th/id/OIP-C.rDZ6Ftzz2FOktLdrm6TOVgHaHa?rs=1&pid=ImgDetMain&o=7&rm=3'
    },
    {   
        id: 7,username: '天球',gender: '1',role: '0',imagePath: 'https://tse1-mm.cn.bing.net/th/id/OIP-C.4e5HBF0Zu6SLR1LNel3_mwHaHa?w=169&h=180&c=7&r=0&o=7&dpr=1.3&pid=1.7&rm=3'
    },
    {   
        id: 8,username: 'Ave Mujica',gender: '1',role: '0',imagePath: 'https://th.bing.com/th/id/OIP.sibi7TgrawI67AYMkJqtTgHaHa?w=209&h=209&c=7&r=0&o=7&dpr=1.3&pid=1.7&rm=3'
    },
    {   
        id: 9,username: 'KILLKISS',gender: '1',role: '0',imagePath: 'https://th.bing.com/th/id/OIP.sibi7TgrawI67AYMkJqtTgHaHa?w=209&h=209&c=7&r=0&o=7&dpr=1.3&pid=1.7&rm=3'
    }
])

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
</script>
<style scoped>
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