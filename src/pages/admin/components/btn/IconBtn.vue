//最起码用户管理和判题服务器都被这个影响，这个是咋回事？？？不太明白，我再看一下
//个人感觉……就是很难说。
//嗷，我知道了，这玩意是个组件，也就是说他就是一个模块，这种东西怎么说呢，可以改，但是改的时候你最好小心点。或者改的时候先去网上 查一下这玩意大概四干什么的
<template>
  <div style="display: inline-block;" class="back">
    
    <el-tooltip class="item back" effect="dark" :content="name" placement="top">
      <el-button plain :icon="'el-icon-fa-' + icon" size="mini">
      </el-button>
    </el-tooltip>
  </div>
</template>

<script>

  export default {
    name: 'IconBtn',
    props: {
      name: {
        type: String,
        required: true
      },
      icon: {
        type: String,
        required: true
      }
    }
  }
  
</script>

<style>
.back
{ background-color: rgba(0, 255, 255, 0.5); 
  transition: background-color 0.3s; 
  position: relative; 
  overflow: hidden; 
}

.back:hover
{ background-color: rgba(0, 255, 255, 0.7); 
}

.back::after{ content: ""; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); width: 200%; height: 200%; background-color: rgba(255, 255, 255, 0.3); border-radius: 50%; transition: all 0.5s; pointer-events: none; }

.back:hover::after{ width: 500%; height: 500%; opacity: 0; }

</style>