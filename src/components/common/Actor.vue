<template>
    <div class="act-out">
        <div id="actor" class="actor"
             @touchmove.native="onTouchMove"
             @touchend.native="onTouchEnd">
            <div class="act-cell" v-if="!show">
                <img :src="director.avatar" alt="导演">
                <div class="act-text">
                    <div>{{director.name}}</div>
                    <div>导演</div>
                </div>
            </div>
            <div class="act-cell" v-for="item in actorList" :key="item.id">
                <img :src="item.avatar" alt="演员">
                <div class="act-text">
                    <div>{{item.name}}</div>
                    <div>演员</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Actor",
        props:['actorList','director'],
        data(){
            return{
                show:false,
                clickTimer: null,
            }
        },
        methods:{
            changeShow(){
                this.show = this.show !== true;
            },
            onTouchMove(e){
                let drag_item = document.getElementById('actor');
                drag_item.style.left = e.touches[0].clientX + 'px';
                drag_item.style.top = e.touches[0].clientY + 'px';
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
    }
    .act-cell{
        width: 25%;
        display: flex;
        flex-direction:column;
    }
    .act-cell img{
        margin: 5%;
    }
    .act-text{
        font-size: 0.3rem;
    }
</style>
