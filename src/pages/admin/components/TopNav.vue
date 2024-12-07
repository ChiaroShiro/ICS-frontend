<template>
  <div class="breadcrumb">
    <div style="background-color: green; width: 200px; height: 300px; margin: 0 auto; z-index: 999;"></div>
<div style="background-color: green; width: 200px; height: 300px; margin: 0 auto; z-index: 999;"></div>

    <el-breadcrumb separator=">">
      <el-breadcrumb-item :to="{ path: '/' }">Home page</el-breadcrumb-item>
      <el-breadcrumb-item><slot name="topNavName">PLEASE OVERIDE ME</slot></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
</template>

<style scoped>
  .breadcrumb {
    margin: 10px;
    margin-right: 25px;
    margin-bottom: 20px;
    padding: 15px;
    background-color: #fff;
  }
</style>
