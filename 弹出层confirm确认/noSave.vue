<template>
    <transition name="confirm-fade">
        <div class="confirm" v-show="showFlag" @click.stop>
            <div class="confirm-wrapper">
                <div class="confirm-content">
                    <h2>{{ confirmText }}</h2>
                    <p>是否确认放弃？</p>
                    <div class="operate">
                        <button @click="toNext" class="operate-btn left">确认</button>
                        <button @click="toBack" class="operate-btn">取消</button>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script type="text/ecmascript-6">
/**
 *传递confirmText参数为弹出确认信息等，
 *父组件直接通过$refs调用confirmShow()函数显示组件
 *点击确认父组件返回一个select自定义事件
 */
export default {
    props: {
        confirmText: {
            type: String,
            default: "放弃后将不在保存进度！"
        },
        confirmBtnNext: {
            type: String,
            default: "确认"
        },
        confirmBtnBack: {
            type: String,
            default: "取消"
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
    background-color: rgba(0, 0, 0, 0.24);
    .confirm-wrapper {
        position: absolute;
        top: 50%;
        left: 50%;
        width: 300px;
        height: 180px;
        border-radius: 5px;
        background: #f5f4f4;
        border: 1px solid rgb(121, 119, 119);
        transform: translate(-50%, -50%);
        padding: 30px;
        box-sizing: border-box;
        .confirm-content {
            h2 {
                font-size: 16px;
                font-weight: 500;
                margin-bottom: 20px;
            }
            p {
                font-size: 14px;
                margin-bottom: 30px;
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
