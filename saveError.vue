<template>
  <transition name="confirm-fade">
    <div class="confirm" v-show="showFlag" @click.stop>
      <div class="confirm-wrapper">
        <div class="confirm-content">
          <h2>检查结果</h2>
          <div class="content">
            <p>错误({{errorList.length}}个)</p>
            <ul>
              <li v-for="(item, index) in errorList" :key="index">
                <span>{{index+1}}、</span>
                {{item}}
              </li>
            </ul>
            <div class="operate">
              <button @click="toNext" class="operate-btn left">返回修改</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
export default {
    props: {
        errorList: {
            type: Array,
            default: ["错误1", "错误2"]
        }
    },
    data() {
        return {
            showFlag: false // v-show标志
        }
    },
    methods: {
        // 组件显示
        confirmShow() {
            this.showFlag = true
        },
        // 组件隐藏
        confirmHide() {
            this.showFlag = false
        },
        // 取消callback
        toBack() {
            this.confirmHide()
        },
        // 确认callback
        toNext() {
            this.confirmHide()
            this.$emit("select")
        }
    }
}
</script>

<style scoped lang="scss">
.confirm {
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    z-index: 19980;
    color: #4d4d4d;
    background-color: rgba(131, 130, 130, 0.24);
    .confirm-wrapper {
        position: absolute;
        top: 50%;
        left: 50%;
        width: 600px;
        border-radius: 5px;
        background: #f5f4f4;
        border: 1px solid rgb(170, 169, 169);
        transform: translate(-50%, -50%);
        box-sizing: border-box;
        h2 {
            width: 100%;
            height: 50px;
            font-size: 16px;
            font-weight: 500;
            background: #499bf1;
            text-indent: 20px;line-height: 50px;
            color: #fff;
        }
        .content {
            padding: 20px 30px;
            overflow: auto;
            p {
                font-size: 14px;
                margin-bottom: 30px;
            }
            .operate {
                float: right;
                margin-top: 5px;
            }
            .operate-btn {
                width: 100px;
                height: 30px;
                border-radius: 5px;
                outline: none;
                background: #499bf1;
                color: #fff;
                cursor: pointer;
            }
        }
    }
}

.confirm-fade-enter-active {
    animation: confirm-fadein 0.3s;
}

.confirm-content {
    animation: confirm-zoom 0.3s;
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
