<template>
  <div class="code-editor">
    <codemirror v-model="code" :options="cmOptions" style="width:100%;height:100%"></codemirror>
  </div>
</template>
<script>
import { codemirror } from 'vue-codemirror'
import 'codemirror/lib/codemirror.css'
// 编辑器语言，暂时只引入.yaml，如需使用更多语言需要先引入相应的js文件
import 'codemirror/mode/yaml/yaml.js'
// 编辑器主题，使用更多主题需引入相应的css文件
import 'codemirror/theme/base16-light.css'
// 光标，当前行高亮
import 'codemirror/addon/selection/active-line.js'
// 自动关闭括号引号
import 'codemirror/addon/edit/closebrackets.js'
export default {
  components: {
    codemirror
  },
  props: {
    //  编辑语言
    codeType: {
      type: String,
      default: 'text/x-yaml'
    },
    // 编辑器主题
    codeTheme: {
      type: String,
      default: 'base16-light'
    },
    // 编辑器传入文本
    codeText: {
      type: String,
      default: ''
    },
    // 编辑器是否设为只读模式，true为只读
    readOnly: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      code: '',
      //  更多相关配置https://codemirror.net/doc/manual.html#config
      cmOptions: {
        autoCloseBrackets: true, // 自动关闭括号引号
        styleActiveLine: true, // 光标当前行高亮
        lineNumbers: true, // 显示呢行号
        line: true,
        mode: this.codeType,
        theme: this.codeTheme,
        readOnly: this.readOnly // 只读
      },
      timer: null
    }
  },
  watch: {
    readOnly(newval) {
      this.cmOptions.readOnly = newval
    },
    //   接受外部传入文本展示在编辑器内
    codeText: {
      handler(newval) {
        if (this.code !== newval) {
          this.code = newval
        }
      },
      immediate: true
    },
    // 编辑器被改变时，对父触发事件传递改变后的文本，500ms防抖
    code(newval) {
      if (this.timer) {
        clearTimeout(this.timer)
      }
      this.timer = setTimeout(() => {
        this.$emit('codeChangeEmit', newval)
      }, 500)
    }
  }
}
</script>
<style lang="scss" scoped>
.code-editor {
  width: 100%;
  height: 100%;
}
</style>
<style lang="scss">
.code-editor {
  .CodeMirror {
    height: 100%;
  }
}
</style>

