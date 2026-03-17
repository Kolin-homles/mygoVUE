<template>
<div>
    <div class="red" v-if="flag==1" >
        红色
    </div>
    <div class="green" v-else-if="flag==2" >
        绿色
    </div>
    <div class="yellow" v-show="flag==3" >
        黄色
    </div>
    <input type="text" v-model.number="flag" >

    <div class="for" v-for="(value,index) in flag" :key="index"> 
        {{value}} 
    </div>
</div>
</template>
<script lang="ts" setup>
import { ref } from 'vue'
const flag = ref(1)
</script>
<style scoped>
.green,.red,.yellow{
    width: 100px;
    height: 100px;
}
.for{
    width: 100%;
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: black;
    color: white;
}
</style>