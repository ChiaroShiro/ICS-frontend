<!-- //登录完了之后退出的时候，会有一瞬间的变化，那个瞬间就是这个页面。 -->
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
