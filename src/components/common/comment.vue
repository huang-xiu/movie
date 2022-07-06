<template>
    <div class="comm-out">
        <div v-if="show" class="comm-text text">{{text}}</div>
        <div v-else class="text">{{text}}</div>
        <van-button v-if="show" class="comm-btn" plain round type="info" @click="changeShow(1)">展开</van-button>
        <van-button v-else class="comm-btn" plain round type="info" @click="changeShow(0)">收起</van-button>
    </div>
</template>

<script>
    export default {
        name: "comment",
        props:['text'],
        data(){
            return{
                show:true,
            }
        },
        methods:{
            changeShow(val){
                this.show = val !== 1;
            }
        }
    }
</script>

<style scoped>
    .comm-out{
        margin: 6px 0 20px 0;
        font-size: 14px;
    }
    .comm-text{
        word-break: break-all;
        text-overflow: ellipsis;
        overflow: hidden;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 4;
    }
    .text{
        margin: 0 5%;
        font-size: 0.35rem;
    }
    .comm-btn{
        margin: 2% 5% 2% 70%;
    }
</style>
