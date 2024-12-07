//前言，这个app.vue是一个整体的底色，就是所有页面都会以这些为底色，这个就不要动了。容易出事。如果改了的话那么记得改回正常版本
//那个版本信息的东西在这个Home里面，等大家决定好的话看看要不要改成什么
<template>
  <div id="app" class="background">
    <router-view></router-view>
      <div class="footer">
        <p>制作人：计科导论第九小组</p>
      </div>
  </div>

</template>

<script>
  export default {
    name: 'app',
    components: {}
  }
</script>

<style lang="less">
 body {
  margin: 0;
  padding: 0;
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "微软雅黑", Arial, sans-serif;
  font-size: 14px;
  -webkit-font-smoothing: antialiased;
  background-image: url('./back.jpg');
  height: 100vh;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

  #app {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
  }
.footer {
  text-align: center;
}

.footer p {
  font-family: 'Segoe Script', cursive; /* 使用带有倒影的艺术字体，可以根据实际情况选择其他艺术字体 */
  font-size: 16px; /* 让汉字变大一号 */
  font-weight: bold; /* 加粗 */
  color: red; /* 红色字体 */
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5); /* 添加文字阴影效果，使其看起来有倒影 */
}
</style>
