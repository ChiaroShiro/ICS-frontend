<template>
  <el-form>
    <el-form-item :label="$t('m.Input')" class="back">
      <el-input type="textarea" v-model="input" @change="changeInput" @keyup.enter.native="changeInput"></el-input>
    </el-form-item>

    <el-form-item :label="$t('m.Output')"class="back">
    </el-form-item>
    <div v-html="text"></div>
  </el-form>
</template>

<script>
  import katex from 'katex'
  export default {
    name: '',
    data () {
      return {
        input: 'c = \\pm\\sqrt{a^2 + b^2}',
        text: ''
      }
    },
    mounted () {
      this.text = this.renderTex(this.input)
    },
    methods: {
      renderTex (data) {
        return katex.renderToString(data, {
          displayMode: true,
          throwOnError: false
        })
      },
      changeInput () {
        try {
          this.text = this.renderTex(this.input)
        } catch (e) {
          this.text = '<p style="text-align: center"><span style="color:red">Error Input</span></p>'
        }
      }
    }
  }
</script>

<style scoped>
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
