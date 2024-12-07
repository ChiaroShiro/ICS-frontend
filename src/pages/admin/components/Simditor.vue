<template>
  <textarea ref="editor" class="back">

  </textarea>
</template>

<script>
  import Simditor from 'tar-simditor'
  import 'tar-simditor/styles/simditor.css'
  import 'tar-simditor-markdown'
  import 'tar-simditor-markdown/styles/simditor-markdown.css'
  import './simditor-file-upload'

  export default {
    name: 'Simditor',
    props: {
      toolbar: {
        type: Array,
        default: () => ['title', 'bold', 'italic', 'underline', 'fontScale', 'color', 'ol', 'ul', '|', 'blockquote', 'code', 'link', 'table', 'image', 'uploadfile', 'hr', '|', 'indent', 'outdent', 'alignment', '|', 'markdown']
      },
      value: {
        type: String,
        default: ''
      }
    },
    data () {
      return {
        editor: null,
        currentValue: this.value
      }
    },
    mounted () {
      this.editor = new Simditor({
        textarea: this.$refs.editor,
        toolbar: this.toolbar,
        pasteImage: true,
        markdown: false,
        upload: {
          url: '/api/admin/upload_image/',
          params: null,
          fileKey: 'image',
          connectionCount: 3,
          leaveConfirm: this.$i18n.t('m.Uploading_is_in_progress')
        },
        allowedStyles: {
          span: ['color']
        }
      })
      this.editor.on('valuechanged', (e, src) => {
        this.currentValue = this.editor.getValue()
      })
      this.editor.on('decorate', (e, src) => {
        this.currentValue = this.editor.getValue()
      })

      this.editor.setValue(this.value)
    },
    watch: {
      'value' (val) {
        if (this.currentValue !== val) {
          this.currentValue = val
          this.editor.setValue(val)
        }
      },
      'currentValue' (newVal, oldVal) {
        if (newVal !== oldVal) {
          this.$emit('change', newVal)
          this.$emit('input', newVal)
        }
      }
    }
  }
</script>

<style lang="less" scoped>
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
