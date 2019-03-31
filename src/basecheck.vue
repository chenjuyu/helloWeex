<template>
    <div class="demo"  ref="cjy" :style="cityExtendStyle">

            <div class="margin">
                <wxc-checkbox-list :list="list"
                                   @wxcCheckBoxListChecked="wxcCheckBoxListChecked"></wxc-checkbox-list>
                <text class="checked-text">选中项 {{checkedList.toString()}}</text>
            </div>

        <wxc-button text="确定"
                    @wxcButtonClicked="wxcButtonClicked"></wxc-button>
    </div>
</template>

<script>
    import { WxcCheckbox,WxcCheckboxList,WxcButton,Utils  } from 'weex-ui'
    export default {
        components:{WxcCheckbox, WxcCheckboxList,WxcButton},
       data() {
        return {
            name: "basecheck",
            list: [
                { title: '选项1', value: 1 },
                { title: '选项2', value: 2, checked: true },
                { title: '选项3', value: 3 },
                { title: '选项4', value: 4 }
            ],
            checkedList: [2]
        }
       }, props: {
            show:{
                default:true
            },
            animationType: {
                type: String,
                default: 'push'
            }

        },

        computed: {
            cityExtendStyle () {
                return Utils.uiStyle.pageTransitionAnimationStyle(this.animationType)
            }

        },mounted(){

        },

        methods: {
            wxcCheckBoxListChecked (e){
                this.checkedList = e.checkedList;
            }, wxcButtonClicked (e) {
                //this.alert(this.checkedList)
                this.show(false);
                this.$emit('choice', { item: this.checkedList.toString() });
                console.log(e)
            }, show(status = true, callback = null) {
                const ref = this.$refs.cjy
                if (this.animationType === 'push') {
                    Utils.animation.pageTransitionAnimation(ref, `translateX(${status ? -750 : 750}px)`, status, callback)
                } else if (this.animationType === 'model') {
                    Utils.animation.pageTransitionAnimation(ref, `translateY(${status ? -Utils.env.getScreenHeight() : Utils.env.getScreenHeight()}px)`, status, callback)
                }
            }
        }
    }
</script>

<style scoped>

    .demo {
        position: fixed;
        width: 750px;
        background-color: #F2F3F4;
    }

</style>