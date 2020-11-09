<template>
    <section 
        class="collapse" 
        ref="collapse" 
        :style="{height: containerH + 'px', 'padding-right': (hideControll ? 0 : 50) + 'px'}"
    >
        <div class="collapse-content" ref="content">
            <slot></slot>
        </div>
        <span v-if="!hideControll" v-show="controllVisible" class="collapse-controll" @click="handleControll">
            <i v-show="!hideControllIcon" class="collapse-controll-icon" :class="isShow ? 'el-icon-arrow-up' : 'el-icon-arrow-down'"></i>
            {{isShow ? foldText : unfoldText}}
        </span>
    </section>
</template>

<script>
export default {
    name: 'collapse',

    model: {
        prop: 'visible',
        event: 'change'
    },

    props: {
        visible: Boolean,

        // 默认显示的内容高度
        defaultHeight: {
            type: Number,
            default: 100
        },

        // 是否隐藏内置控制按钮
        hideControll: Boolean,

        // 是否隐藏内置控制按钮icon
        hideControllIcon: Boolean,

        foldText: {
            type: String,
            default: '收起'
        },

        unfoldText: {
            type: String,
            default: '展开'
        }
    },

    data(){
        return {
            isShow: false,
            controllVisible: true,

            containerH: this.defaultHeight,
            contentH: 0,
            DOMWatcher: null,
        }
    },

    watch: {
        visible(v){
            if(v){
                this.unfold();
            }else{
                this.fold();
            }
        },

        isShow(v){
            this.$emit('change', v);
        }
    },

    mounted(){
        this.init();
        this.watch();

        if(this.visible){
            this.unfold();
        }
    },

    destroyed(){
        this.DOMWatcher.disconnect();
    },

    methods: {
        init(){
            this.contentH = this.$refs.content.offsetHeight;
            /**
             * 当内容变化的同时也是展开状态的话，那么需要更新容器高度
             * 1. 当最新的内容高度大于默认高度时，则把容器高度更新最新内容高度一致即可
             * 2. 反之，把容器高度设到最小（即默认高度），同时设为收起状态
             */
            if(this.isShow){
                if(this.contentH > this.defaultHeight){
                    this.containerH = this.contentH;
                }else{
                    this.containerH = this.defaultHeight;
                    this.isShow = false;
                }
            }
            this.controllVisible = this.contentH > this.defaultHeight; // 控制按钮的显示隐藏
        },

        watch(){
            this.DOMWatcher = new MutationObserver(() =>{
                // 监测到DOM变化，重新计算高度
                this.refresh();
            })
            this.DOMWatcher.observe(this.$refs.content, {
                childList: true,
                attributes: true,
                characterData: true,
                subtree: true
            })
        },

        refresh(){
            this.$nextTick(() => {
                this.init();
            })
        },

        handleControll(){
            let handle = this.isShow ? this.fold : this.unfold;
            handle();
        },

        // 收起
        fold(){
            this.containerH = this.defaultHeight;
            this.$nextTick(() => {
                this.isShow = false;
            })
        },

        // 展开
        unfold(){
            const contentH = this.$refs.content.offsetHeight;
            if(contentH > this.defaultHeight){
                this.containerH = contentH;
            }
            this.$nextTick(() => {
                this.isShow = true;
            })
        },
    }
}
</script>

<style scoped>
.collapse{
    width: 100%;
    position: relative;
    overflow: hidden;
    transition: height .28s ease-in-out;
}
.collapse-controll{
    position: absolute;
    bottom: 0px;
    right: 8px;
    font-size: 14px;
    font-weight: 400;
    color: #6ABCFF;
    cursor: pointer;
}
.collapse-controll-icon{
    margin-right: 2px;
}
</style>