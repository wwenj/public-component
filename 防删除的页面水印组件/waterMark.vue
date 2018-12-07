<template>
<div></div>
</template>
<script>
export default {
  props: {
    // 显示的水印文本
    inputText: {
      type: String,
      default: 'waterMark水印'
    },
    // 是否允许通过js或开发者工具等途径修改水印DOM节点（水印的id，attribute属性，节点的删除）
    // true为允许，默认不允许
    inputAllowDele: {
      type: Boolean,
      default: false
    },
    // 是否在组件销毁时去除水印节点（前提是允许用户修改DOM，否则去除后会再次自动生成）
    // true会，默认不会
    inputDestroy: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      maskDiv: {} // 当前显示的水印div节点DOM对象
    }
  },
  mounted () {
    // 确认DOM渲染后再执行
    this.$nextTick(() => {
      // 创建水印节点
      this.init()
      if (!this.inputAllowDele) {
        // 设置水印节点修改的DOM事件
        this.Monitor()
      }
    })
  },
  methods: {
    init () {
      let canvas = document.createElement('canvas')
      canvas.id = 'canvas'
      canvas.width = '200' // 单个水印的宽高
      canvas.height = '130'
      this.maskDiv = document.createElement('div')
      let ctx = canvas.getContext('2d')
      ctx.font = 'normal 18px Microsoft Yahei' // 设置样式
      ctx.fillStyle = 'rgba(112, 113, 114, 0.1)' // 水印字体颜色
      ctx.rotate(30 * Math.PI / 180) // 水印偏转角度
      ctx.fillText(this.inputText, 30, 20)
      let src = canvas.toDataURL('image/png')
      this.maskDiv.style.position = 'fixed'
      this.maskDiv.style.zIndex = '9999'
      this.maskDiv.id = '_waterMark'
      this.maskDiv.style.top = '30px'
      this.maskDiv.style.left = '0'
      this.maskDiv.style.height = '100%'
      this.maskDiv.style.width = '100%'
      this.maskDiv.style.pointerEvents = 'none'
      this.maskDiv.style.backgroundImage = 'URL(' + src + ')'
      // 水印节点插到body下
      document.body.appendChild(this.maskDiv)
    },
    Monitor () {
      let body = document.getElementsByTagName('body')[0]
      let options = {
        childList: true,
        attributes: true,
        characterData: true,
        subtree: true,
        attributeOldValue: true,
        characterDataOldValue: true
      }
      let observer = new MutationObserver(this.callback)
      observer.observe(body, options) // 监听body节点
    },
    // DOM改变执行callback
    callback (mutations, observer) {
      // 当attribute属性被修改
      if (mutations[0].target.id === '_waterMark') {
        this.removeMaskDiv()
      }
      // 当id被改变时
      if (mutations[0].attributeName === 'id') {
        this.removeMaskDiv()
        this.init()
      }
      // 当节点被删除
      if (mutations[0].removedNodes[0] && mutations[0].removedNodes[0].id === '_waterMark') {
        this.init()
      }
    },
    /* public */
    // 手动销毁水印DOM
    removeMaskDiv () {
      document.body.removeChild(this.maskDiv)
    },
    // 手动生成水印
    createMaskDiv () {
      this.init()
    }
  },
  watch: {
    // 监听传入水印文本变化
    inputText () {
      this.$nextTick(() => {
        this.removeMaskDiv()
      })
    }
  },
  destroy () {
    // 组件销毁时去除生成在body节点下的水印节点
    if (this.inputDestroy) {
      this.removeMaskDiv()
    }
  }
}
</script>
