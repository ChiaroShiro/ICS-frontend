//登录完了之后退出的时候，会有一瞬间的变化，那个瞬间就是这个页面。
//这个页面不需要懂，那一瞬间的东西根本看不出来。不需要做特效。全是浪费。
<template>
</template>

<script>


  import api from '../../api.js'

  export default {
    mounted () {
      api.logout().then(res => {
        this.$store.dispatch('clearProfile')
        this.$router.replace({
          path: '/'
        })
      })
    }
  }
</script>
