<template>
  <el-button plain type="primary" class = "back">{{$t('m.Cancel')}}</el-button>
</template>
<script>
  export default{
    name: 'Cancel'
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