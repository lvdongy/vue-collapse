<template>
    <div style="padding: 50px">
        <section class="box">
            <collapse :default-height="70">
                <div v-for="i in 6" :key="i">这是内容: {{i}}</div>
            </collapse>
        </section>

        <!-- <section class="box">
            <collapse v-model="isOpen" :default-height="70">
                <div v-for="i in 6" :key="i">这是内容: {{i}}</div>
            </collapse>
        </section> -->

        <section class="box">
            <collapse v-model="isOpen" :default-height="70" hide-controll>
                <div v-for="i in 6" :key="i">这是内容: {{i}}</div>
            </collapse>
            <span class="btn" @click="isOpen = !isOpen">{{isOpen ? '关' : '开'}}</span>
        </section>
    </div>
</template>

<script>
import collapse from '@/components/collapse/index'
export default {
    name: 'demo',

    components: {
        collapse
    },

    data(){
        return {
            isOpen: true,
        }
    }
}
</script>

<style lang='scss' scope>
.box{
    width: 200px;
    padding: 16px;
    margin-bottom: 50px;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    border-radius: 6px;
    color: #666666;
    font-size: 14px;
    line-height: 24px;
    position: relative;
    overflow: hidden;
}
.btn{
    background: #6ABCFF;
    color: #fff;
    font-size: 12px;
    border-radius: 50%;
    cursor: pointer;
    position: absolute;
    width: 60px;
    height: 60px;
    padding-left: 12px;
    padding-top: 31px;
    box-sizing: border-box;

}
</style>