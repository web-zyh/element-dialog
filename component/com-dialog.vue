<template>
     <div>
        <el-dialog
            :width="dialogWidth"
            :visible.sync="visible"
            :visibleDialog="visibleDialog"
            @close="handleClose"
            :before-close = "handleBeforeClose"
            :close-on-click-modal="false"
            :title="title"
            :center="center"
            :buttonTexts="buttonTexts">
            <slot></slot>
            <div slot="footer" class="dialog-footer">
                <el-button :type="buttonLeftType" :size="buttonSize" @click="handleleftButton">{{buttonTexts[0]}}</el-button>
                <el-button :type="buttonRightType" :size="buttonSize" @click="handleRightButton">{{buttonTexts[1]}}</el-button>
            </div>
        </el-dialog>
    </div>
</template>
<script>
    export default{
        data(){
            return {
                dialogWidth: "",
                visible:this.visibleDialog
            }
        },
        props: {
            title:{
                type: String,
                default: "提示"
            },
            center:{
                type: Boolean,
                default: true
            },
            buttonTexts:{
                type: Array, //[Array]
                default:()=>{
                    return ["取消","确定"]
                }
            },
            buttonLeftType:{ // primary / success / warning / danger / info / text
                type: String,
                default:()=>{
                    return "info"
                }
            },
            buttonRightType:{ // primary / success / warning / danger / info / text
                type: String,
                default:()=>{
                    return "primary"
                }
            },
            buttonSize:{ // medium / small / mini
                type: String,
                default:()=>{
                    return "medium"
                }
            },
           
            visibleDialog: {
                type: Boolean,
                default: false
            },
        },
        created() {
            this.setDialogWidth();
        },
        mounted(){
            window.onresize = () => {
                return (() => {
                    this.setDialogWidth();
                })();
            };
        },
        methods: {
            setDialogWidth() {
                let wdh = document.documentElement.clientWidth;
                const defWdh = 768; // 默认宽度
                if (wdh < defWdh) {
                    this.dialogWidth = "85vw";
                } else {
                    this.dialogWidth = "50vw";
                }
            },
            handleClose(){
                this.$emit('update:visibleDialog',false);//当dialog关闭时更新父组件show的状态为false，如果不添加这句，父组件第一次打开之后show就会一直处于show:true的状态
                this.$emit('close');
            },
            handleBeforeClose(done){
                this.$emit('update:visibleDialog',false);//当dialog关闭时更新父组件show的状态为false，如果不添加这句，父组件第一次打开之后show就会一直处于show:true的状态
                this.$emit('before-close',done);
            },
            handleleftButton(){
                this.$emit('handleleftButton');
            },
            handleRightButton(){
                this.$emit('handleRightButton');
            },
        },
        watch: {
            visibleDialog () {
                this.visible = this.visibleDialog;
            }
        }
    }
    
</script>