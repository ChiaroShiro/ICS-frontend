<template>
<div>
  <div v-if="showContent" class="container">
    <div>
      <SideMenu></SideMenu>
    </div>
  
    <div id="header" class="header">
      <div class="background-img"></div>
      <i class="el-dropdown el-icon-fa-font katex-editor" @click="katexVisible=true"></i>
      <screen-full :width="14" :height="14" class="el-dropdown screen-full"></screen-full>

      <el-dropdown @command="handleCommand">
        <span>{{user.username}}<i class="el-dropdown el-icon-caret-bottom el-icon--right"></i></span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item command="logout">Logout</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>

      <div class="floating-image-container"></div>
    </div>
  
    <div class="content-app">
      <transition name="fadeInUp" mode="out-in">
        <router-view></router-view>
      </transition>

    </div>

    <el-dialog :title="$t('m.Latex_Editor')" :visible.sync="katexVisible">
      <KatexEditor></KatexEditor>
    </el-dialog>
  </div>

<button @click="showContent = true" v-show="!showContent" class="show-button">
  <span class="button-text">TJ-OJ</span>
</button>

</div>
</template>

<script>
import { types } from '@/store'
import { mapGetters } from 'vuex'
import SideMenu from '../components/SideMenu.vue'
import ScreenFull from '@admin/components/ScreenFull.vue'
import KatexEditor from '@admin/components/KatexEditor.vue'
import api from '../api'

export default {
  name: 'app',
  data () {
    return {
      version: process.env.VERSION,
      katexVisible: false,
      showContent: false
    }
  },
  components: {
    SideMenu,
    KatexEditor,
    ScreenFull
  },
  beforeRouteEnter (to, from, next) {
    api.getProfile().then(res => {
      if (!res.data.data) {
        next({name: 'login'})
      } else {
        next(vm => {
        vm.$store.commit(types.CHANGE_PROFILE, {profile: res.data.data})
      })
    }
  })
},
methods: {
  handleCommand (command) {
    if (command === 'logout') {
      api.logout().then(() => {
        this.$router.push({name: 'login'})
      })
    }
  }
},
computed: {
  ...mapGetters(['user'])
}
}
</script>

<style lang="less">
a {
  background-color: transparent;
}

a:active, a:hover {
  outline-width: 0
}
img {
  border-style: none;
}

.el-dropdown {
  position: relative;
  z-index: 9999;
}
 .button-text {
    font-size: 24px; /* 设置字体大小为24像素 */
    font-weight: bold; /* 设置字体加粗 */
    color: red; /* 设置字体颜色为红色 */
    font-family: 'Arial Black', sans-serif; /* 设置艺术字体 */
    margin-left: 5px;
}
.container {
  overflow: auto;
  font-weight: 400;
  height: 100%;
  -webkit-font-smoothing: antialiased;
  background-color: #EDECEC;
  overflow-y: scroll;
  min-width: 1000px;
}

* {
  box-sizing: border-box;
}

#header {
  position: relative;
  text-align: right;
  overflow: hidden;
  padding-left: 210px;
  padding-right: 30px;
  line-height: 50px;
  height: 60px;
}

.background-img {
  position: absolute;
  top: 0;
  left: 0;
  width: 200%;
  height: 100%;
  background: url('./cloud.png') repeat;
  background-size: auto;
  animation: slide 60s linear infinite;
  z-index: 0;
  border-radius: 20px;
  background-blend-mode: multiply;
}

@keyframes slide {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: 100% 0;
  }
}

.background-img:hover {
  animation-play-state: paused;
}

.content-app {
  padding-top: 20px;
  padding-right: 10px;
  padding-left: 210px;
}

.footer {
  margin: 15px;
  text-align: center;
  font-size: small;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translate(0, 30px);
  }

  to {
    opacity: 1;
    transform: none;
  }
}

.fadeInUp-enter-active {
  animation: fadeInUp .8s;
}

.katex-editor {
  margin-right: 5px;
} .show-button {
    height: 200px;
    width: 200px;
    padding: 15px;
    border: none;
    cursor: pointer;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.3);
    overflow: hidden; /* 将超出部分裁剪掉，使元素变成圆形 */
}

.show-button::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    transition: transform 0.3s;
    z-index: -1;
}

.show-button:hover::before {
    background: rgba(0, 255, 0, 1); /* 绿色完全不透明的背景 */
    transform: scale(2); /* 鼠标悬停时微微放大 */
}



</style>