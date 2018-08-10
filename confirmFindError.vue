<template>
  <transition name="confirm-fade">
    <div class="confirm" v-show="showFlag" @click.stop>
      <div class="confirm-wrapper">
        <div class="confirm-content">
          <h2>纠错</h2>
          <div class="content">
            <label><input type="checkbox" v-model="isCheckbox" @click="checkClick('findError')" name="chose" data-index="33">
              <span>录入有误</span>
            </label>
            <textarea name="chose" placeholder="请输入错误信息，必填" v-model="textarea"></textarea>
            <div class="operate">
              <button @click="toNext" class="operate-btn left">{{confirmBtnNext}}</button>
              <button @click="toBack" class="operate-btn">{{confirmBtnBack}}</button>
            </div>
          </div>
        </div>
      </div>
      <find-error-alert ref="findErrorAlert"></find-error-alert>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
import findErrorAlert from "./confirmFindErrorAlert"
export default {
    components: {
        findErrorAlert
    },
    props: {
        confirmBtnNext: {
            type: String,
            default: "完成"
        },
        confirmBtnBack: {
            type: String,
            default: "取消"
        }
    },
    data() {
        return {
            showFlag: false, // v-show标志
            textarea: "", // 文本框内容
            checkbox: 0, // 输入内容
            isCheckbox: false // 判断复选框状态
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
            this.isCheckbox = false
            this.textarea = ""
        },
        // 确认callback
        // 判断内容不为空与复选框选中才能继续
        toNext() {
            if (this.textarea === "" || !this.isCheckbox) {
                this.$refs.findErrorAlert.show() // 不符合条件弹出
                return
            }
            this.confirmHide()
            let con = {
                type: this.checkbox,
                text: this.textarea
            }
            this.$emit("select", con)
        },
        // 纠错类型获取
        checkClick(value) {
            if (!this.isCheckbox) {
                this.checkbox = value
            } else {
                this.checkbox = 0
            }
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
    background-color: rgba(0, 0, 0, 0.24);
    .confirm-wrapper {
        position: absolute;
        top: 50%;
        left: 50%;
        width: 400px;
        height: 250px;
        border-radius: 5px;
        background: #f5f4f4;
        border: 1px solid rgb(121, 119, 119);
        transform: translate(-50%, -50%);
        h2 {
            font-size: 16px;
            font-weight: 500;
            height: 50px;
            line-height: 50px;
            text-indent: 20px;
            border-bottom: 1px solid black;
        }
        .content {
            width: 100%;
            padding: 20px;
            box-sizing: border-box;
            input {
                margin-left: 5px;
            }
            span {
                font-size: 14px;
                cursor: pointer;
                // margin-left: 5px;
            }
            textarea {
                width: 100%;
                height: 100px;
                margin-top: 10px;
                outline: none;
                box-sizing: border-box;
                padding: 5px;
            }
            .operate {
                float: right;
                margin-top: 5px;
            }
            .operate-btn {
                width: 70px;
                height: 25px;
                border-radius: 5px;
                outline: none;
                cursor: pointer;
            }
            .left {
                margin-right: 15px;
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
