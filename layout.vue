<template>
<div class="layout">
    <div class="header">
        头部
    </div>
    <div class="section">
        <div class="left">
            左边
        </div>
        <div class="right">
            右边
        </div>
        <div class="main">
            中间
        </div>
    </div>
    <div class="footer">
        底部 
    </div>   
</div>
</template>
<script lang="ts" setup>
</script>
<style scoped>
.layout{
    display:flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    border: 1px solid red#880c0c;
    background: #ccc;
}
.header,.footer{
    width: 100%;
    height: 60px;
    background: pink;
}
.section{
    flex:1;
    background: #eee;
}
.main{
    flex:1;
    background: #ccc;
    
}

</style>