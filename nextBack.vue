<template>
  <transition name="drop">
    <div class="toAlert" v-show="showFlag">
      <div class="toAlert-text">{{alertText}}</div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
export default {
    props: {
        alertText: {
            type: String,
            default: "获取上一题成功"
        },
        delay: {
            type: Number,
            default: 2000
        }
    },
    data () {
        return {
            showFlag: false
        }
    },
    methods: {
        show () {
            this.showFlag = true
            clearTimeout(this.timer)
            this.timer = setTimeout(() => {
                this.hide()
            }, this.delay)
        },
        hide () {
            this.showFlag = false
        }
    }
}
</script>

<style scoped lang="scss">
.toAlert {
    position: fixed;
    top: 50%;
    left: 50%;
    width: 200px;
    height: 100px;
    transform: translate(-50%, -50%);
    border-radius: 8px;
    z-index: 111900;
    background: rgb(91, 169, 241);
}
.toAlert-text {
    text-align: center;
    line-height: 100px;
    color: #ffff;
    font-size: 14px;
}
.drop-enter {
    opacity: 0;
}
.drop-enter-active {
    opacity: 1;
}
</style>
