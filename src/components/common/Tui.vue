<template>
    <div class="act-out">
        <div class="actor">
            <div class="act-cell" v-for="item in tuiList" :key="item.id">
                <img :src="item.cover" alt="图片">
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Tui",
        props:['tuiList'],
        data(){
            return{
                show:false,
                clickTimer: null,
            }
        },
        methods:{
            changeShow(){
                this.show = this.show !== true;
            }
        }
    }
</script>

<style scoped>
    .act-out{
        display:flex;
        flex-shrink:0;
        padding: 0 2%;
    }
    .act-btn{
        background-color: #aeaeae;
        top:0; bottom:0; left:0; right:0; margin:auto;
        padding: 5px;
        font-size: 0.2rem;
        width: 3%;
    }
    .actor{
        flex-shrink:1;
        flex-grow: 1;
        display: flex;
        overflow-x: scroll;
        height: 4rem;
    }
    .act-cell{
        display: flex;
        flex-direction:column;
        width: 45%;
        height: 100%;
    }
    .act-cell img{
        margin: 5%;
        height: 93%;
    }
</style>
