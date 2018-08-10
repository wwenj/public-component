<template>
  <transition name="drop">
    <div class="toAlert" v-show="showFlag">
      <div class="toAlert-text">{{alertText}}</div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
export default {
    /**
     * 父组件直接通过$refs调用show()函数显示组件，显示后delay秒消失
     * 父组件用alertText传递弹出信息，用delay传递消失时间
     * **/
    props: {
        alertText: {
            type: String,
            default: "错误原因不能为空！！"
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
    // animation: confirm-fadein 0.3s;
    // animation: confirm-zoom 0.3s;
}
@keyframes confirm-fadein {
    0% {
        opacity: 0;
    }

    100% {
        opacity: 1;
    }
}

@keyframes confirm-zoom {
    0% {
        transform: scale(0);
    }

    50% {
        transform: scale(1.1);
    }

    100% {
        transform: scale(1);
    }
}
</style>
