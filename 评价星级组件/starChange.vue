<template>
    <div>
        <ul class="star-ul">
            <li class="star-li" v-for="(item,index) in starList" :key="index" @click="starClick(index)">
                <img v-if="starShow(index)" src="../../../../assets/img/star1.jpg" alt="评星">
                <img v-else src="../../../../assets/img/star2.jpg" alt="评星">
            </li>
        </ul>
    </div>
</template>

<script type="text/ecmascript-6">
export default {
    /**
     * 点击评星，返回父组件评星星级数
     */
    props: {
        starList: {
            type: Array,
            default: [0, 1, 2, 3, 4]
        }
    },
    data() {
        return {
            starIndex: -1
        }
    },
    computed: {},
    methods: {
        starClick: function(index) {
            this.starIndex = index
            index += 1
            this.$emit("select", index)
        },
        starShow: function(index) {
            if (index <= this.starIndex) {
                return false
            } else {
                return true
            }
        }
    }
}
</script>

<style scoped lang="scss">
.star-ul {
    display: inline-block;
    overflow: auto;
    .star-li {
        float: left;
        margin-right: 10px;
        cursor: pointer;
    }
}
</style>
