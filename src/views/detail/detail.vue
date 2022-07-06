<template>
    <div>
        <HeadTop head-title="电影详情" go-back="true" ></HeadTop>
        <!-- 简介 -->
        <div class="description">
            <img class="img desc-img" :src="detail.cover" alt="cover">
            <div class="desc-con">
                <div class="desc-title">{{detail.name}}（{{detail.showDate.substr(0,4)}}）</div>
                <div class="desc-text0">{{timeInMinite}}分钟/{{typeName}}/{{region}}</div>
                <div class="desc-text1">上影评分：{{detail.tuiScore}}分&emsp;{{detail.reviewTimes}}人评分</div>
                <div class="desc-text2">豆瓣评分：{{detail.doubanScore}}分</div>
                <div class="clear">
                    <div class="lf desc-text1">观看渠道：{{watchRoad}}</div>
                    <div class="lf desc-link">前往</div>
                </div>
            </div>
        </div>
        <!-- 吸顶栏 -->
        <van-sticky>
            <div class="menu">
                <div class="menu-item" @click="goView('desc')">电影介绍</div>
                <div class="menu-item" @click="goView('comm')">上影推荐</div>
                <div class="menu-item" @click="goView('disc')">影评讨论</div>
            </div>
        </van-sticky>
        <!-- 电影介绍 -->
        <div id="desc">
            <div>
                <comment :text="detail.desc"></comment>
            </div>
        </div>
        <!-- 演职员 -->
        <div class="row">
            <div class="part-title">演职员</div>
            <actor :actorList="actorList" :director="director"></actor>
        </div>
        <!-- 上影推荐（结构同电影介绍 -->
        <div class="row" id="comm">
            <div class="part-title">上影推荐</div>
            <div>
                <comment :text="detail.tuiDesc"></comment>
            </div>
        </div>
        <!-- 上影推剪辑、图 -->
        <div class="row">
            <div class="part-title">上影推剪辑</div>
            <tui :tuiList="tuiVideos"></tui>
        </div>
        <div class="row">
            <div class="part-title">上影推图</div>
            <tui :tuiList="tuiImages"></tui>
        </div>
        <!-- 观看渠道 -->
        <div class="row">
            <div class="part-title">观看渠道</div>
            <div class="watch-text" v-if="!watchType">暂无片源</div>
            <div v-else>
                <div v-if="watchType===1||watchType===3" class="clear">
                    <div class="watch-title lf">第三方</div>
                    <div v-for="i in 3" :key="watchList[i-1].movieWatch.platformId">
                        <a class="watch-text lf" v-if="watchList[i-1].platform.id" :href="watchList[i-1].platform.icon">{{watchList[i-1].platform.name}}</a>
                    </div>
                </div>
                <div v-if="watchType===2||watchType===3" class="clear">
                    <div class="watch-title lf">BT</div>
                    <a class="watch-text lf" :href="third.movieWatch.url">{{third.movieWatch.url}}</a>
                </div>
            </div>
        </div>
        <!-- 评论区 -->
        <div class="row" id="disc">
            <div class="part-title">评论区</div>
        </div>
    </div>
</template>

<script>
    import HeadTop from '@/components/header/Head';
    import comment from '@/components/common/comment';
    import Actor from '@/components/common/Actor';
    import Tui from '@/components/common/Tui';

    export default {
        name: "detail",
        data(){
            return{
                //接口返回值
                detail:{
                    "id": 2,
                    "name": "狮子王真人版",
                    "type": 31,
                    "cover": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/904f062ca9ae309fa02bd5937547f8ce.jpg",
                    "status": 1,
                    "desc": "广袤无垠的非洲大草原，祥和奋进的荣耀王国迎来了光辉时刻。威严而勇敢的狮子国王木法沙（詹姆斯·厄尔·琼斯 James Earl Jones 配音）与王后沙拉碧（阿尔法·伍达德 Alfre Woodard 配音），终于迎来了他们的儿子——王国未来的王者辛巴（唐纳德·格洛弗 Donald Glover 配音）——的诞生。在所有的动物都心悦诚服跪拜辛巴王子之际，只有木法沙的弟弟刀疤（切瓦特·埃加福 Chiwetel Ejiofor 配音）妒火中烧。他久久觊觎哥哥的王位，而今更是将辛巴视为眼中钉肉中刺。经过一番策划，刀疤害死了木法沙，并将罪名推给了侄子，进而将王位攫取到自己手中。\r\n　　荣耀王国从此被黑暗笼罩，而遭到流放的辛巴是否还能回到这个他度过快乐童年的国度呢？",
                    "tuiDesc": "如果说25年前，动画电影被《狮子王》重新定义了，25年后的它又要重新定义电影拍摄了吧。一部没有真实动物、任何演员，甚至没有摄影机的电影，完全复刻着最真实的电影拍摄，而且相比下来更没有妥协。比起那些狂轰滥炸观感的特效，它甚至显得太低调了，这也正是它最革命之处了。“真兽版”注定比动画版少了那些极致的形式主义，但细节和角色都处理的更丰满了，能带给观众的情绪也更足了。刀疤和娜娜两个角色得到更完整的延伸，这点比前作要好，至于彭彭和丁满两个被疯狂加戏的活宝，让我觉得都可以出外传了！全片最神的一场戏，当属辛巴的狮毛漂流记",
                    "tuiImages": "\"\"",
                    "tuiVideos": "\"\"",
                    "showDate": "2020-05-17 00:00:00",
                    "regions": "1,2",
                    "directorId": 1,
                    "directorName": "杨荔钠",
                    "actorIds": "1,2",
                    "actorNames": "郝蕾,金燕玲,曲隽希",
                    "tuiScore": 8.0,
                    "doubanScore": 7.2,
                    "collectTimes": 100,
                    "reviewTimes": 15,
                    "addTime": "2020-06-17 08:15:41",
                    "updateTime": "2020-06-17 08:15:43"},
                actorList:[{
                    "id": 1,
                    "name": "王宝强",
                    "avatar": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/101f2f4758b240af956f0c6ac528fa3b.png",
                    "status": 0,
                    "desc": "好演员",
                    "addTime": null,
                    "updateTime": null
                },{
                    "id": 1,
                    "name": "王宝强",
                    "avatar": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/101f2f4758b240af956f0c6ac528fa3b.png",
                    "status": 0,
                    "desc": "好演员",
                    "addTime": null,
                    "updateTime": null
                }, {
                    "id": 1,
                    "name": "王宝强",
                    "avatar": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/101f2f4758b240af956f0c6ac528fa3b.png",
                    "status": 0,
                    "desc": "好演员",
                    "addTime": null,
                    "updateTime": null
                }, {
                    "id": 1,
                    "name": "王宝强",
                    "avatar": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/101f2f4758b240af956f0c6ac528fa3b.png",
                    "status": 0,
                    "desc": "好演员",
                    "addTime": null,
                    "updateTime": null
                }],
                watchList: [
                    {
                        "movieWatch": {
                            "id": 0,
                            "movieId": 2,
                            "type": 1,
                            "platformId": 1,
                            "url": "1111",
                            "status": 0,
                            "addTime": null,
                            "updateTime": null
                        },
                        "platform": {
                            "id": 3,
                            "name": "腾讯视频",
                            "icon": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/20220620222439.png",
                            "status": 0,
                            "addTime": null,
                            "updateTime": null
                        }
                    }, {
                        "movieWatch": {
                            "id": 0,
                            "movieId": 2,
                            "type": 1,
                            "platformId": 2,
                            "url": "222",
                            "status": 0,
                            "addTime": null,
                            "updateTime": null
                        },
                        "platform": {
                            "id": 0,
                            "name": null,
                            "icon": null,
                            "status": 0,
                            "addTime": null,
                            "updateTime": null
                        }
                    }, {
                        "movieWatch": {
                            "id": 0,
                            "movieId": 2,
                            "type": 1,
                            "platformId": 3,
                            "url": "333",
                            "status": 0,
                            "addTime": null,
                            "updateTime": null
                        },
                        "platform": {
                            "id": 0,
                            "name": null,
                            "icon": null,
                            "status": 0,
                            "addTime": null,
                            "updateTime": null
                        }
                    }, {
                        "movieWatch": {
                            "id": 0,
                            "movieId": 2,
                            "type": 2,
                            "platformId": 0,
                            "url": "SDFGHUJKJHGFDSDFGHJKJHGFDFGHJSDFGHJ",
                            "status": 0,
                            "addTime": null,
                            "updateTime": null
                        },
                        "platform": null
                    }
                ],//观看渠道
                director:{
                    "id": 1,
                    "name": "李思诚",
                    "avatar": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/343deaa70a544f738a765db3e52137fd.png",
                    "status": 0,
                    "desc": "好导演",
                    "addTime": null,
                    "updateTime": null
                },
                tuiVideos:[{
                    "id": 0,
                    "movieId": 2,
                    "status": 0,
                    "type": 1,
                    "rank": 1,
                    "cover": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/271c8b728df646f8a8c5b1c0921087a6.png",
                    "videoUrl": "1235.mp4",
                    "zanTimes": 100,
                    "addTime": null,
                    "updateTime": null
                }, {
                    "id": 0,
                    "movieId": 2,
                    "status": 0,
                    "type": 1,
                    "rank": 1,
                    "cover": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/271c8b728df646f8a8c5b1c0921087a6.png",
                    "videoUrl": "1235.mp4",
                    "zanTimes": 100,
                    "addTime": null,
                    "updateTime": null
                }],//推电影对象列表，按rank排序
                tuiImages:[{
                    "id": 0,
                    "movieId": 2,
                    "status": 0,
                    "type": 2,
                    "rank": 1,
                    "cover": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/661503557ec8419eb94ef482132cf36c.png",
                    "videoUrl": null,
                    "zanTimes": 10,
                    "addTime": null,
                    "updateTime": null
                }, {
                    "id": 0,
                    "movieId": 2,
                    "status": 0,
                    "type": 2,
                    "rank": 1,
                    "cover": "http://intern-test.oss-cn-hangzhou.aliyuncs.com/image/m/661503557ec8419eb94ef482132cf36c.png",
                    "videoUrl": null,
                    "zanTimes": 10,
                    "addTime": null,
                    "updateTime": null
                }],//推电影图片列表，按rank排序
                typeName:'喜剧',//电影类型
                timeInMinite:'128',//时长，按分钟计
                collected:true,//当前登录用户是否收藏
                //处理得到
                region:'中国大陆',
                watchRoad:'第三方平台',
                watchType:3,//0无渠道，1有第三方，2有BT，3都有
                third:{
                    "movieWatch": {
                        "id": 0,
                        "movieId": 2,
                        "type": 2,
                        "platformId": 0,
                        "url": "SDFGHUJKJHGFDSDFGHJKJHGFDFGHJSDFGHJ",
                        "status": 0,
                        "addTime": null,
                        "updateTime": null
                    },
                    "platform": null
                },
            }
        },
        components:{
            HeadTop,comment,Actor,Tui
        },
        methods:{
            goView(id){
                document.getElementById(id).scrollIntoView();
            }
        }
    }

</script>

<style scoped>
    .lf{float: left;}
    .rt{float: right;}
    .clear{clear: both;}
    img{}
    .description{display: flex;}
    .desc-img{
        width: 30%;
        margin: 2%;
        flex: 0.3;
    }
    .desc-con{
        width: 50%;
        margin-left: 5%;
        flex: 0.7;
    }
    .desc-title{
        margin: 6% 0;
        font-size: 1rem;
        word-break: break-all;
        text-overflow: ellipsis;
        overflow: hidden;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 2;
    }
    .desc-text0{
        font-size: 0.35rem;
        margin: 5% 0;
    }
    .desc-text1{
        font-size: 0.35rem;
    }
    .desc-text2{
        font-size: 0.35rem;
        color: #787d82;
    }
    .desc-link{
        font-size: 0.35rem;
        color: #42b983;
        margin-left: 3%;
    }
    .row{
    }
    .part-title{
        font-size: 0.5rem;
        margin: 3% 5%;
    }
    .menu{
        display: flex;
        margin: 2% 5%;
        background-color: white;
    }
    .menu-item{
        width: 33.3%;
        flex: 1;
        text-align: center;
        font-size: 0.35rem;
        padding: 1%;
        border: 1px solid #555555;
    }
    .watch-title{
        background-color: #cbcbcb;
        width: 1rem;
        height: 1rem;
        font-size: 0.3rem;
        margin: 1% 3%;
        text-align: center;
    }
    .watch-text{
        margin: 0.4rem 2% 0;
        font-size: 0.2rem;
    }
</style>
