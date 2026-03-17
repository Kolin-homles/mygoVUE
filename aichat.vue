<template>
    <div class = "layout">
        <div class="ai-header">
            AI智慧助手
        </div>
        <div class="ai-section">
            <div v-for="(item,index) in messages" :key="index">
               <div class="robot" v-if="item.name=='ai猫猫酱'">
                    <img class ="tx" src="../assets/cat2.jpg" alt="头像" width="50px" height="50px">
                    <div class = "robot-right">
                        <div class = "robot-name">{{item.name}}</div>
                        <div class = "robot-chat">{{item.message}}</div>
                    </div>
                </div>

                <div  class="me" v-else>
                    <div class="me-left">
                        <div class = "me-name">{{item.name}}</div>
                        <div class = "me-message">{{item.message}}</div>
                    </div>
                    <img class ="tx" src="../assets/soyo.jpg" alt="头像" width="50px" height="50px">
                </div>
            </div>
        </div>
        <div class="ai-footer">
            <el-input
                v-model="input"
                placeholder="Please input"
            >
            </el-input>
            <el-button type="success" @click="submit">发送</el-button>
            <el-dialog
            v-model="dialogVisible"
            title="Tips"
            width="500"
            >
            </el-dialog>
        </div>
    </div>
</template>
<script lang="ts" setup>

import { ref } from 'vue';
const input = ref('泥嚎');

const dialogVisible = ref(false);
const messages = ref([
{   
    name:'ai猫猫酱',
    message:'你好，我是AI智慧neko酱，请问有什么可以帮助您的吗？'
},
{
    name:'soyo',
    message:'你好，为什么要演奏春日影！！！！！'
}

])

const submit=()=>{
    messages.value.push({
        name:'soyo',
        message:input.value
    })
}
</script>
<style scoped>
.layout{
    display: flex;
    flex-direction: column;
    height: 100%;
    width: 100%;
}
.ai-header{
    height: 55px;
    background: #e5c0e1;
}
.ai-section{
    flex: 1;
    background: #84d6f1;
   
    border-radius: 15px;
    margin-bottom: 5px;
}
.ai-footer{
    height: 80px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #afbc1f;
}
.robot{
    display: flex;
    padding: 10px;
}
.robot-right{
    margin-left: 10px;
}
.tx{
    width: 50px;
    height: 50px;
    border-radius: 50%;
}
.robot-name{
    font-weight: bold;
    margin-bottom: 5px;
}
.robot-chat{
    background: #fff;
    border-radius: 15px;
    padding: 10px;
    max-width: 450px;
    word-wrap: break-word;
    word-break: break-all;
    white-space: normal;
}
.me{
    display: flex;
    justify-content: flex-end;
    padding: 10px;
}
.me-left{
    width: 450px;
    margin-right: 10px;
}
.me-name{
    display: flex;
    justify-content: flex-end;
}
.me-message{
   background: white;
   max-width: 400px;
   padding: 10px;
   word-wrap: break-word;
   word-break: break-all;
   white-space: normal;
   border-radius: 15px;
}
</style>