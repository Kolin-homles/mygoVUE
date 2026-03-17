<template>
  <div class="common-layout">
    <el-container style="height: 100%; width: 100%;">
      <el-header class="header" style="padding: 0px;"> 
        <div class="et-header"> 
            <div class="header-left">
                <img src="../assets/mygo2.webp" alt="" class="width"
                style="width: 50px; height: 50px; ">
                <h2>MyGo!!!!!</h2>
            </div>
            <div class="header-right">
                <!-- ========== 修改开始：添加欢迎文本 ========== -->
                <!-- 在图片前添加显示欢迎语的span元素 -->
                <span class="welcome-text">{{ welcomeText }}</span>
                <!-- 为图片添加class以便样式控制 -->
                <img src="../assets/cat.jpg" alt="用户头像" class="width avatar"
                style="width: 60px; height: 60px; ">
                <!-- ========== 修改结束 ========== -->
            </div>
            
        </div>  
      </el-header>
      <el-container>
      <el-aside width="249px" class="left">
        <div class="et-left">
            <el-menu
                default-active="/home/user"
                class="el-menu-vertical-demo"
                router
                >
                <el-menu-item index="/home/user">
                    <el-icon><UserFilled /></el-icon>
                    <span>（菜单1）成员管理</span>
                </el-menu-item>

                <el-menu-item index="/home/item">
                    <el-icon><CopyDocument /></el-icon>
                    <span>（菜单2）歌曲管理</span>
                </el-menu-item>

                <el-menu-item index="/home/aichat">
                    <el-icon><Comment /></el-icon>
                    <span>（菜单3）智慧助手</span>
                </el-menu-item>
            </el-menu>
        </div>
      </el-aside>
      <el-main class="main" style="padding: 0px;">
        <div class="et-main">
          <router-view></router-view>
        </div>
      </el-main>

      </el-container>
    </el-container>
  </div>
</template>

<script lang="ts" setup>
// ========== 修改开始：导入必要的库 ==========
import {
  CopyDocument,
  UserFilled,
  Comment
} from '@element-plus/icons-vue'
import { ro } from 'element-plus/es/locale/index.mjs';

// 导入路由相关功能
import { useRouter, useRoute } from 'vue-router'
// 导入Vue响应式功能
import { ref, computed, onMounted } from 'vue'
// ========== 修改结束 ==========

// ========== 修改开始：获取路由信息 ==========
// 注意：router用于跳转，route用于获取当前路由信息
const router = useRouter()
const route = useRoute()  // 新增：用于获取路由参数
// ========== 修改结束 ==========

// ========== 修改开始：定义响应式变量 ==========
// 存储用户名的响应式变量
const username = ref('')

// 计算欢迎文本，根据当前时间显示不同的问候语
const welcomeText = computed(() => {
  if (!username.value) {
    return '请先登录'
  }
  
  // 获取当前小时数
  const hour = new Date().getHours()
  let greeting = ''
  
  // 根据时间段设置不同的问候语
  if (hour >= 5 && hour < 12) {
    greeting = '早上好'
  } else if (hour >= 12 && hour < 18) {
    greeting = '下午好'
  } else {
    greeting = '晚上好'
  }
  
  return `${greeting}，${username.value}`
})
// ========== 修改结束 ==========

// ========== 修改开始：在组件挂载时获取用户名 ==========
onMounted(() => {
  // 1. 首先尝试从路由参数获取用户名
  if (route.query.username) {
    username.value = route.query.username as string
    // 保存到本地存储，防止刷新页面后丢失
    localStorage.setItem('username', username.value)
  } 
  // 2. 如果路由参数中没有，尝试从本地存储获取
  else {
    const storedUsername = localStorage.getItem('username')
    if (storedUsername) {
      username.value = storedUsername
    }
  }
})
// ========== 修改结束 ==========
</script>

<style scoped>
.header-right{
    width: 400px;
    height: 100%;
    border-radius: 5px;
    /* ========== 修改开始：调整header-right的布局 ========== */
    display: flex;               /* 使用flex布局 */
    align-items: center;         /* 垂直居中 */
    justify-content: flex-end;   /* 内容靠右对齐 */
    gap: 15px;                   /* 子元素之间的间距 */
    /* ========== 修改结束 ========== */
}
.header-left{
    width: 249px;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 5px;
}
.header{
  height: 70px;
}  
.et-header{
  background: white;
  height: 90%;
  width: 98%;
  box-shadow: 5px 10px 5px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.common-layout{
    width: 100%;
    height: 100%;
}
.left{
   
}
.et-left{
    background: white;
    height: 96%;
    width: 95%;
    box-shadow: 5px 10px 5px rgba(0, 0, 0, 0.3);
    border-radius: 10px;
}
.main{
    background: yellowgreen;
}
.et-main{
    background: white;
    height: 97%;
    width: 98%;
    box-shadow: 5px 10px 5px rgba(0, 0, 0, 0.3);
    border-radius: 10px;
}
.header,.left,.main{
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 5px;
}

/* ========== 修改开始：新增样式规则 ========== */
/* 欢迎文本的样式 */
.welcome-text {
  font-size: 16px;               /* 字体大小 */
  font-weight: 500;              /* 字体粗细，500为中等 */
  color: #333;                   /* 字体颜色，深灰色 */
  padding: 5px 10px;             /* 内边距，上下5px，左右10px */
  border-radius: 5px;            /* 圆角边框 */
  background-color: rgba(240, 248, 255, 0.8); /* 半透明浅蓝色背景 */
  border: 1px solid #c8e1ff;     /* 浅蓝色边框 */
  transition: all 0.3s ease;     /* 所有属性变化时有0.3秒的过渡效果 */
  white-space: nowrap;           /* 防止文字换行 */
}

/* 鼠标悬停时的效果 */
.welcome-text:hover {
  background-color: rgba(224, 240, 255, 0.9); /* 悬停时背景色变深 */
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);  /* 添加阴影 */
  transform: translateY(-1px);               /* 向上微移1像素，产生浮动效果 */
}

/* 头像样式 */
.avatar {
  border-radius: 50%;            /* 圆形头像 */
  border: 2px solid #409eff;     /* 蓝色边框，与Element Plus主题色一致 */
  transition: all 0.3s ease;     /* 过渡效果 */
}

/* 鼠标悬停在头像上的效果 */
.avatar:hover {
  transform: scale(1.05);         /* 放大5% */
  box-shadow: 0 0 10px rgba(64, 158, 255, 0.5); /* 添加发光效果 */
}
/* ========== 修改结束 ========== */
</style>