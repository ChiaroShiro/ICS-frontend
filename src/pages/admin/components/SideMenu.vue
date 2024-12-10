<template>
  <el-menu class="vertical_menu"
           :router="true" :default-active="currentPath">
    <div class="logo">
      <img src="../../../assets/tongji.png" alt="同济大学欢迎您"/>
    </div>
    <el-menu-item index="/" class="ahead"><i class="el-icon-fa-dashboard"></i>{{$t('m.Dashboard')}}</el-menu-item>
    <el-submenu v-if="isSuperAdmin" index="general" >
      <template slot="title"><i class="el-icon-menu"></i>{{$t('m.General')}}</template>
      <el-menu-item index="/user">{{$t('m.User')}}</el-menu-item>
      <el-menu-item index="/announcement">{{$t('m.Announcement')}}</el-menu-item>
    </el-submenu >
    <el-submenu index="problem" v-if="hasProblemPermission">
      <template slot="title"><i class="el-icon-fa-bars"></i>{{$t('m.Problem')}}</template>
      <el-menu-item index="/problems">{{$t('m.Problem_List')}}</el-menu-item>
      <el-menu-item index="/problem/create">{{$t('m.Create_Problem')}}</el-menu-item>

    </el-submenu>
    <el-submenu index="contest">
      <template slot="title"><i class="el-icon-fa-trophy"></i>{{$t('m.Contest')}}</template>
      <el-menu-item index="/contest">{{$t('m.Contest_List')}}</el-menu-item>
      <el-menu-item index="/contest/create">{{$t('m.Create_Contest')}}</el-menu-item>
    </el-submenu>
  </el-menu>
</template>

<script>
  import {mapGetters} from 'vuex'

  export default {
    name: 'SideMenu',
    data () {
      return {
        currentPath: ''
      }
    },
    mounted () {
      this.currentPath = this.$route.path
    },
    computed: {
      ...mapGetters(['user', 'isSuperAdmin', 'hasProblemPermission'])
    }
  }
</script>

<style scoped lang="less">


.el-menu-item{
  background-color: rgba(236, 223, 238, 0.3)!important;
  font-weight: bold;
  font-size: 13px !important; /* 设置字号为原来的1.2倍 */
  color: lightcoral; /* 设置字体颜色为淡红色 */
}
.el-submenu {
  background-color:rgba(221, 154, 230, 0.3);
  font-weight: bold;
  font-size: 50px !important; /* 设置字号为原来的1.2倍 */
  color: lightcoral; /* 设置字体颜色为淡红色 */
}
.ahead {
  background-color: rgba(221, 154, 230, 0.3)!important;  /*设置背景颜色并提高优先级 */
  font-weight: bold !important; /* 设置字体加粗并提高优先级 */
  font-size: 100% !important; /* 设置字号为原来的1.2倍 */
  color: lightcoral; /* 设置字体颜色为淡红色 */
}

  .vertical_menu {
    overflow: auto;
    width: 205px;
    height: 100%;
    position: fixed !important;

    z-index: 100;
    top: 0;
    bottom: 0;
    left: 0;
    background-image: url('../../../assets/tj_back2.png');
    background-repeat: no-repeat;
     background-size: cover; 
    background-size: height 100px; /* 根据需要调整背景图片的显示方式 */
    background-color: rgba(255, 255, 255, 0.5); /* 设置半透明背景色 */
    .logo {
      margin: 20px 0;
      text-align: center;
      img {
        background-color:orangered;
        border-radius: 50%;
        border: 3px solid #fff;
        width: 75px;
        height: 75px;
      }
    }
  }
</style>
