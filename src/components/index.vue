<template>
    <div class="index">
        <div class="background"></div>
        <div class="screen" @click="ranking = true" v-if="rankingData[0]">
            <p>恭喜</p>
            <img :src="rankingData[0].wechatHeadimg" alt="头像">
            <p>{{ rankingData[0].wechatNickname }}签到{{ rankingData[0].typeSignCount }}次，位居傍一</p>
        </div>
        <div class="button">
            <div class="btn" v-for="item in info.types" @click="typeChange(item)">{{ item.typeName }}</div>
        </div>
        <div class="text">
            <div class="centianer">
                <div class="title">打卡奖励规则</div>
                <div class="text-p">
<!--                    <p>① 单次购买满30元就可以找老板扫码打卡</p>-->
<!--                    <p>② 首次打卡赠送蛋挞一个</p>-->
<!--                    <p>③ 打卡7次赠送6寸西点</p>-->
<!--                    <p>④ 打卡12次赠送6寸生日蛋糕</p>-->
<!--                    <p>⑤ 打卡22次赠送8寸生日蛋糕</p>-->
<!--                    <p>⑥ 长按保存打卡二维码，下次就能直接使用手机相册里二维码图片打卡</p>-->
                    <p>{{ info.ruleName }}</p>
                </div>
            </div>
            <div class="image">
                <img :src="info.qrCode" alt="">
                <p>长按保存二维码</p>
            </div>
            <div class="label">直接让老板扫码就可以完成打卡</div>
        </div>
        <div class="reward">
            <div class="top">摩尼卡蛋糕店卡签</div>
            <div class="center">
                <div class="reward-center">
                    <swiper :options="swiperOption" ref="mySwiper">
                        <!-- slides -->
                        <swiper-slide :key="index" v-for="(item,index) in arrayItem">
                            <div class="reward-content" >
                                <div class="image-item" v-for="(itemItem,indexItem) in item.array">
                                    <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="itemItem.type && (rewardItem.typeSignCount < indexItem+1+15*index)" @click="reward(itemItem,'not')" alt="">
                                    <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="itemItem.type && (rewardItem.typeSignCount >= indexItem+1+15*index)" class="shake" @click="reward(itemItem)" alt="">
                                    <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="!itemItem.type && (rewardItem.typeSignCount < indexItem+1+15*index)" alt="">
                                    <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="!itemItem.type  && (rewardItem.typeSignCount >= indexItem+1+15*index)" alt="">
                                </div>
                            </div>
                        </swiper-slide>
                        <!-- Optional controls -->
                        <div class="swiper-button-prev" slot="button-prev"></div>
                        <div class="swiper-button-next" slot="button-next"></div>
                    </swiper>
                </div>
            </div>
            <div class="bottom">已打卡{{ rewardItem.typeSignCount }}次</div>
        </div>
        <!--        奖品-->
        <transition name="fade">
            <div class="model" v-show="model" @click.self="model=false">
                <div class="model-cotent">
                    <div class="model-cole" @click="model=false"></div>
                    <h3>打卡奖品</h3>
                    <p>{{ rewardText }}： {{ rewardCent }} </p>
                </div>
            </div>
        </transition>
        <!--        排行榜-->
        <transition name="fade">
            <div class="model" v-show="ranking" @click.self="ranking=false">
                <div class="ranking-content">
                    <div class="ranking-cole" @click="ranking=false"></div>
                    <div class="top-image" v-if="rankingData[0]">
                        <img :src="rankingData[0].wechatHeadimg" alt="">
                    </div>
                    <div class="top-title" v-if="rankingData[0]">{{ rankingData[0].wechatNickname }}</div>
                    <div class="top-text" v-if="rankingData[0]">第1名：签到 {{ rankingData[0].typeSignCount }} 次</div>
                    <div class="ranking-list">
                        <div class="ranking-list-item" v-for="(item,index) in rankingData">
                            <div class="ranking-top"></div>
                            <div class="ranking-left">
                                <div v-if="index === 0" style="background-color: #D93F46" class="number">1</div>
                                <div v-else-if="index === 1" style="background-color: #74DFE9" class="number">2</div>
                                <div v-else-if="index === 2" style="background-color: #4C9EDF" class="number">3</div>
                                <div v-else style="background-color: #EDF5FA" class="number">{{ index+1 }}</div>
                                <div class="ranking-image">
                                    <img :src="item.wechatHeadimg" alt="">
                                </div>
                                <div class="ranking-name">{{ item.wechatNickname }}</div>
                            </div>
                            <div class="ranking-right">
                                签到{{ item.typeSignCount }}次
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>

<script>
    import 'swiper/dist/css/swiper.css'
    import { swiper, swiperSlide } from 'vue-awesome-swiper'
    export default {
        name: 'index',
        components: {
            swiper,
            swiperSlide
        },
        data() {
            return {
                model: false,
                rewardText: '',
                rewardCent: '',
                timeName: '',
                data: {},
                info: {},
                value: '',
                typeId: '',
                rewardItem: {},
                arrayItem:[],
                animationLeft: false,
                animationRight: false,
                // 轮播图配置
                swiperOption: {
                    speed: 600,
                    effect: 'slide',
                    navigation: {
                        nextEl: '.swiper-button-next',
                        prevEl: '.swiper-button-prev'
                    }
                },
                // 排行数据
                ranking: false,
                rankingData: []
            }
        },
        methods: {
            reward(item,type) {
                if (type === 'not') {
                    this.rewardText = '奖品为'
                } else {
                    this.rewardText = '恭喜您获得'
                }
                this.rewardCent = item.prizeName;
                this.model = true;
            },
            getData() {
                if (window.location.search.split("=")[0] === '') {
                    // window.location.href = "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx709ac12a75ab18ee&redirect_uri="+encodeURIComponent(window.location.href.split("?")[0]+window.location.hash)+"&response_type=code&scope=snsapi_userinfo&state=123#wechat_redirect"
                    window.location.href = "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx709ac12a75ab18ee&redirect_uri="+encodeURIComponent(window.location.href.split("?")[0])+"&response_type=code&scope=snsapi_userinfo&state=123#wechat_redirect"
                } else {
                    // let code = window.location.search.split("=")[1].split("&")[0];
                    let code = this.$route.query.code;
                    this.$axios({
                        method: 'post',
                        url: "/appservice/wechat/qrcode/getWechatUserinfo?code=" + code
                    }).then((item) => {
                        let data = item.data;
                        if (data.state === 200) {
                            // this.$jsonp('https://api.weixin.qq.com/sns/userinfo?access_token=27_UaMQ-K1Wu207vZcQqUYxZQO3vyRz9vfGVYfeoSvLfvUft91LVFA299Hj-EP48vN7XFt5lpdfAmUbGETRETZ8Uw&openid=or-mB56uFXpKmxC2whkA8nVncyfY&lang=zh_CN', null,(err,data)=> {
                            //     console.log(err,data)
                            // });
                            // this.$jsonp("https://restapi.amap.com/v3/ip?key=84a2938c865de6340b894054ecce87b7",null,(err,data)=> {
                            //     console.log(data)
                            // });
                            let url = "";
                            if (data.data.nickname) {
                                url = "/appservice/wechat/qrcode/register?wechatAccount="+data.data.openid+"&wechatNickname="+data.data.nickname+"&wechatHeadimg="+data.data.headimgurl
                            } else {
                                url = "/appservice/wechat/qrcode/register?wechatAccount=or-mB56uFXpKmxC2whkA8nVncyfY&wechatNickname=团子的老父亲&wechatHeadimg=http://thirdwx.qlogo.cn/mmopen/vi_32/LDN7icjqGqTUDDVoVvw1VtNNoibbgl6oYoNMgDEjZArCYGpEBGSsSCHChraL0bO3qWcANdzSBop105VjnKyiaMRnA/132"
                            }
                            this.$axios({
                                method: 'post',
                                url: url,
                            }).then((item1) => {
                                let data1 = item1.data;
                                if (data1.state === 200) {
                                    this.data = data1;
                                    this.getUser();
                                }
                            })

                        } else {
                            window.location.href = "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx709ac12a75ab18ee&redirect_uri="+encodeURIComponent(window.location.href.split("?")[0]+window.location.hash)+"&response_type=code&scope=snsapi_userinfo&state=123#wechat_redirect"
                        }
                    });
                }

            },
            getUser() {
                this.getUserInfo();
                // this.timeName = setInterval(() => {
                //     this.getUserInfo();
                // },2000);
            },
            // 用户信息
            getUserInfo() {
                this.$axios({
                    method: 'post',
                    url: "/appservice/wechat/qrcode/getUserCardSign?id="+this.data.data
                }).then((item) => {
                    let data = item.data;
                    if (data.state === 200) {
                        // this.info = data.data;
                        let loading = document.getElementById('Loading');
                        if (loading != null) {
                            document.body.removeChild(loading);
                            this.queryUserinfo();
                        }
                    }
                })
                // this.$axios({
                //     method: 'post',
                //     url: "/appservice/wechat/qrcode/queryUserinfo?id="+this.data.data
                // }).then((item) => {
                //     let data = item.data;
                //     if (data.state === 200) {
                //         this.info = data.data;
                //         let loading = document.getElementById('Loading');
                //         if (loading != null) {
                //             document.body.removeChild(loading);
                //         }
                //     }
                // })
            },
            // 切换卡签类型
            typeChange(item) {
                this.rewardItem = item;
                this.setRewardItem();
            },
            // 卡签用户详情信息
            queryUserinfo() {
                this.$axios({
                    method: 'post',
                    url: "/appservice/wechat/qrcode/queryUserinfo?merchantId=9&id="+this.data.data
                }).then((item) => {
                    let data = item.data;
                    if (data.state === 200) {
                        //=数据===================
                        // data.data = {
                        //     id: 10029,
                        //     level: null,
                        //     password: null,
                        //     qrCode: "https://konkonyu.oss-cn-beijing.aliyuncs.com/qrcode/or-mB56uFXpKmxC2whkA8nVncyfY/二维码.png",
                        //     registerTime: "2019-12-04 12:29:02",
                        //     ruleName: "规则描述",
                        //     typeSignCount: null,
                        //     types: [
                        //         {
                        //             typeId: 10000000,
                        //             typeName: "15天签到",
                        //             merchantId: "9",
                        //             typeSignCount: 30,
                        //             prizeList: [
                        //                 {
                        //                     prizeName: "苹果1",
                        //                     signinCount: 1
                        //                 },
                        //                 {
                        //                     prizeName: "苹果2",
                        //                     signinCount: 2
                        //                 },{
                        //                     prizeName: "苹果3",
                        //                     signinCount: 8
                        //                 },
                        //                 {
                        //                     prizeName: "苹果4",
                        //                     signinCount: 18
                        //                 },{
                        //                     prizeName: "苹果5",
                        //                     signinCount: 21
                        //                 },{
                        //                     prizeName: "苹果6",
                        //                     signinCount: 43
                        //                 }
                        //             ]
                        //         },
                        //         {
                        //             typeId: 10000001,
                        //             typeName: "30天签到",
                        //             merchantId: "10",
                        //             typeSignCount: 9,
                        //             prizeList: [
                        //                 {
                        //                     prizeName: "蛋挞一份",
                        //                     signinCount: 1
                        //                 },
                        //                 {
                        //                     prizeName: "6寸西点一份",
                        //                     signinCount: 7
                        //                 },{
                        //                     prizeName: "8寸西点一份",
                        //                     signinCount: 12
                        //                 },
                        //                 {
                        //                     prizeName: "8寸生日蛋糕一份",
                        //                     signinCount: 22
                        //                 }
                        //             ]
                        //         }
                        //     ],
                        //     wechatAccount: "or-mB56uFXpKmxC2whkA8nVncyfY",
                        //     wechatHeadimg: "http://thirdwx.qlogo.cn/mmopen/vi_32/LDN7icjqGqTUDDVoVvw1VtNNoibbgl6oYoNMgDEjZArCYGpEBGSsSCHChraL0bO3qWb5HSsDg14yb8AKiaia64jTKg/132",
                        //     wechatNickname: "良民。",
                        // };
                        this.info = data.data;
                        this.typeId = data.data.types[0].typeId;
                        this.rewardItem = data.data.types[0];
                        // 处理礼品动态展示
                        this.setRewardItem();
                        // 获取排行榜
                        this.getRankinglist();
                    }
                })
            },
            // 处理礼品动态展示
            setRewardItem() {
                let array = [];
                this.rewardItem.prizeList.forEach((c,index) => {
                    if (index === 0) {
                        for(let i = 0; i < c.signinCount; i++) {
                            if (i === c.signinCount-1) {
                                let objectItem = {
                                    prizeName: c.prizeName,
                                    signinCount: c.signinCount,
                                    type: true
                                };
                                array.push(objectItem);
                            } else {
                                let objectItem = {
                                    prizeName: '',
                                    signinCount: i+1,
                                    type: false
                                };
                                array.push(objectItem);
                            }
                        }
                    } else {
                        for(let i = this.rewardItem.prizeList[index-1].signinCount; i < c.signinCount; i++) {
                            if (i === c.signinCount-1) {
                                let objectItem = {
                                    prizeName: c.prizeName,
                                    signinCount: c.signinCount,
                                    type: true
                                };
                                array.push(objectItem);
                            } else {
                                let objectItem = {
                                    prizeName: '',
                                    signinCount: i+1,
                                    type: false
                                };
                                array.push(objectItem);
                            }

                        }
                    }
                });
                let arrayItem = [];
                array.forEach((c,index) => {
                    if (index === 0) {
                        let object = {
                            array: [c]
                        };
                        arrayItem.push(object)
                    } else if (index === (Math.ceil(index / 15) * 15)) {
                        let object = {
                            array: [c]
                        };
                        arrayItem.push(object)
                    } else {
                        let indexItem = Math.ceil(index / 15) - 1;
                        arrayItem[indexItem].array.push(c)
                    }
                });
                this.arrayItem = arrayItem;
            },
            // 排行榜
            getRankinglist() {
                this.$axios({
                    method: 'post',
                    url: "/appservice/wechat/qrcode/getrankinglist?merchantId=9&typeId="+this.typeId
                }).then((item) => {
                    let data = item.data;
                    if (data.state === 200) {
                        //=数据===================
                        // data.data = [
                        //     {
                        //         "userId": 10007,
                        //         "typeSignCount": 3,
                        //         "wechatNickname": "白云",
                        //         "wechatHeadimg": "http://thirdwx.qlogo.cn/mmopen/vi_32/E6B6c2avOG4ia3taib6tQib6RaaT6VlRI4GxfgF3qCWIzkLqaH49u2VwsZcOPmAXqtNP3MA0H6AeicWHI3iabVnn9WA/132"
                        //     },
                        //     {
                        //         "userId": 10010,
                        //         "typeSignCount": 1,
                        //         "wechatNickname": "简单（程）",
                        //         "wechatHeadimg": "http://thirdwx.qlogo.cn/mmopen/vi_32/hMXgibHyjt8FYnq6tobhNib1ricL8RdQqtN4kiattNSxJ4dL5NtMXMZO0RdrS5lqJ2Sh2RiaOTK0PfNQQO6NT7x6xgg/132"
                        //     },
                        //     {
                        //         "userId": 10024,
                        //         "typeSignCount": 1,
                        //         "wechatNickname": "Xc.",
                        //         "wechatHeadimg": "http://thirdwx.qlogo.cn/mmopen/vi_32/Q0j4TwGTfTLicZsTVhLDTQ4NTeFKibCE7ezfgsZxD0IJxdfDHNRnrsTv91cF3o44UAd7mNbyI7onxyjIQsLdBfRg/132"
                        //     },
                        //     {
                        //         "userId": 10010,
                        //         "typeSignCount": 1,
                        //         "wechatNickname": "简单（程）",
                        //         "wechatHeadimg": "http://thirdwx.qlogo.cn/mmopen/vi_32/hMXgibHyjt8FYnq6tobhNib1ricL8RdQqtN4kiattNSxJ4dL5NtMXMZO0RdrS5lqJ2Sh2RiaOTK0PfNQQO6NT7x6xgg/132"
                        //     },
                        //     {
                        //         "userId": 10024,
                        //         "typeSignCount": 1,
                        //         "wechatNickname": "Xc.",
                        //         "wechatHeadimg": "http://thirdwx.qlogo.cn/mmopen/vi_32/Q0j4TwGTfTLicZsTVhLDTQ4NTeFKibCE7ezfgsZxD0IJxdfDHNRnrsTv91cF3o44UAd7mNbyI7onxyjIQsLdBfRg/132"
                        //     }
                        // ];
                        this.rankingData = data.data;
                    }
                })
            }
        },
        mounted() {
            this.getData();
        },
        beforeDestroy(){
//            清楚定时器
            clearInterval(this.timeName)
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .index {
        /*background: url("https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/background.f9ae2d1.png") no-repeat top center;*/
        background: url("../assets/image/background.png") repeat top center;
        background-size: 100%;
        min-height: 100vh;
        overflow: hidden;
    }
    .background {
        height: 9vh;
        background: url("../assets/image/background1.png") no-repeat top center;
        background-size: 100%;
    }
    .screen {
        overflow: hidden;
        text-align: center;
        color: #2480B4;
        background: rgba(105,211,241,.5);
        padding: 5px 0;
    }
    .screen img {
        width: 5vw;
        vertical-align: middle;
        border-radius: 5vw;
    }
    .screen p {
        display: inline-block;
        vertical-align: middle;
    }
    .button {
        overflow: hidden;
        padding: 10px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .button .btn {
        background: url("../assets/image/button.png") no-repeat center center;
        background-size: 100%;
        width: 30vw;
        height: 11.5vw;
        line-height: 11.5vw;
        text-align: center;
        margin: 0 3vw;
        color: #fff;
    }
    .text {
        width: 90%;
        margin: 0 auto;
        background: url("https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/text-background.7889ec7.png") no-repeat top center;
        background-size: 100%;
        padding: 2vw;
    }
    .text .centianer {
        background: #fff;
        border-top-left-radius: 30px;
        border-top-right-radius: 30px;
        height: 55.6vw;
        /*overflow-y: auto;*/
    }
    .centianer .text-p {
        height: 43vw;
        overflow-y: auto;
    }
    .title {
        background: #449FE3;
        color: #fff;
        font-size: 16px;
        font-weight: 900;
        width: 140px;
        text-align: center;
        margin: -1px auto 16px;
        padding: 4px 0;
        border-bottom-left-radius: 10px;
        border-bottom-right-radius: 10px;
    }
    .centianer p {
        color: #56A8E6;
        padding: 0 10%;
        line-height: 24px;
    }
    .image {
        width: 33vw;
        margin: 20px auto 0;
        background: #62D0EE;
        text-align: center;
        border-radius: 1em;
        overflow: hidden;
    }
    .image img {
        display: block;
        width: 100%;
        padding: 10px;
        background: #fff;
    }
    .image p {
        display: block;
        padding: .5em 0;
        font-size: 10px;
        font-weight: 600;
        color: #fff;
    }
    .label {
        color: #fff;
        text-align: center;
        padding: 14px 0;
        font-weight: 600;
    }
    .reward {
        width: 90%;
        height: 74vw;
        margin: 30px auto;
        background: url("https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/reward-background.a287592.png") no-repeat top center;
        background-size: 100%;
        padding: 8px;
        position: relative;
    }
    .reward .top {
        position: absolute;
        top: -14px;
        left: 50%;
        margin-left: -68px;
        background: #DB454E;
        color: #fff;
        font-size: 16px;
        font-weight: 900;
        padding: 6px 10px;
        border: 2px solid #fff;
        border-radius: 14px;
    }
    .center {
        padding-top: 10vw;
        padding-bottom: 2.6vw;
        overflow-y: hidden;
    }
    .center .reward-center {
        float: left;
        width: 100%;
        height: 42vw;
        overflow: hidden;
        position: relative;
    }
    .reward-content {
        padding: 0 10%;
        height: 42vw;
    }
    /*改变了颜色和加粗的样式*/
    .swiper-button-prev{
        background:url("../assets/image/left.png") no-repeat center left;
        background-size: 40%;
    }
    .swiper-button-next{
        background:url("../assets/image/rigth.png") no-repeat center right;
        background-size: 40%;
    }

    .reward-content .image-item {
        float: left;
        display: block;
        width: 20%;
    }

    .reward-content .image-item img {
       width: 100%;
    }
    .reward-content {
        overflow: hidden;
    }
    .bottom {
        background: #D83D44;
        font-size: 16px;
        font-weight: 600;
        padding: 8px;
        color: #fff;
        text-align: center;
        width: 60%;
        margin: 0 auto;
        border-radius: 16px;
        border: 2px solid #E6B9BE;
    }
    .model {
        width: 100vw;
        height: 100vh;
        background: rgba(0,0,0,.5);
        position: fixed;
        top: 0;
        left: 0;
        z-index: 999;
    }
    /*奖励展示*/
    .model-cotent {
        width: 70%;
        height: 70vw;
        background: url("../assets/image/model.png") no-repeat center center;
        background-size: 100%;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        text-align: center;
        padding: 14vw 0;
    }
    .model-cotent h3 {
        font-weight: 900;
        font-size: 18px;
    }
    .model-cotent p {
        font-size: 15px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
    }
    .model-cole {
        width: 8vw;
        height: 30vw;
        background: url("../assets/image/cole.png") no-repeat center bottom;
        background-size: 100%;
        position: absolute;
        right: 10vw;
        top: -29vw;
    }
    /*排行榜*/
    .ranking-content {
        width: 88%;
        height: 120vw;
        background: url("../assets/image/ranking.png") no-repeat center top;
        background-size: 100%;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        text-align: center;
    }
    .ranking-cole {
        width: 6vw;
        height: 30vw;
        background: url("../assets/image/cole.png") no-repeat center bottom;
        background-size: 100%;
        position: absolute;
        right: 10vw;
        top: -18vw;
    }
    /*头像*/
    .top-image {
        padding-top: 3vw;
    }
    .top-image img {
        width: 24%;
        border-radius: 50%;
    }
    /*名字*/
    .top-title {
        font-weight: 900;
        font-size: 16px;
        padding: 1.4vw 0;
    }
    .top-text {
        font-weight: 500;
        padding: 2vw 0 5vw;
    }
    /*列表*/
    .ranking-list {
        height: 75vw;
        overflow-y: auto;
        border-bottom-right-radius: 11vw;
        border-bottom-left-radius: 11vw;
    }
    .ranking-list-item {
        width: 100%;
        display: flex;
        align-items: center;
        padding: 3vw 0;
        position: relative;
    }
    .ranking-top {
        position: absolute;
        top: 0;
        left: 50%;
        transform: translateX(-50%);
        width: 90%;
        border-bottom: 1px solid #eee;
    }
    .ranking-left {
        display: flex;
        align-items: center;
    }
    .ranking-right {
        position: absolute;
        right: 5vw;
        color: #FF971C;
        font-weight: 600;
    }
    .number {
        float: left;
        color: #fff;
        font-weight: 900;
        padding: .6vw 2.6vw .4vw;
        border-top-right-radius: 3vw;
        border-bottom-right-radius: 3vw;
    }
    .ranking-image {
        float: left;
        width: 13vw;
        margin: 0 2vw;
    }
    .ranking-image img {
        width: 100%;
        display: block;
        border-radius: 12vw;
    }
    .ranking-name {
        float: left;
        font-weight: 900;
    }
    .ranking-right {
        float: right;
    }
    /**/
    .fade-enter-active{
        transition: opacity .5s;
    }
    .fade-enter {
        opacity: 0;
    }
    .shake {
        animation: unpacking .5s;
        -moz-animation: unpacking .5s;	/* Firefox */
        -webkit-animation: unpacking .5s;	/* Safari 和 Chrome */
        -o-animation: unpacking .5s;
    }
    @keyframes unpacking {
        0% {
            transform: translate(0px, 0px) rotate(0deg);
        }
        2% {
            transform: translate(-1px, 1.5px) rotate(1.5deg);
        }
        4% {
            transform: translate(1.3px, 0px) rotate(-0.5deg);
        }
        6% {
            transform: translate(1.4px, 1.4px) rotate(-2deg);
        }
        8% {
            transform: translate(-1.3px, -1px) rotate(-1.5deg);
        }
        10% {
            transform: translate(1.4px, 0px) rotate(-2deg);
        }
        12% {
            transform: translate(-1.3px, -1px) rotate(-2deg);
        }
        14% {
            transform: translate(1.5px, 1.3px) rotate(1.5deg);
        }
        16% {
            transform: translate(1.5px, -1.5px) rotate(-1.5deg);
        }
        18% {
            transform: translate(1.3px, -1.3px) rotate(-2deg);
        }
        20% {
            transform: translate(1px, 1px) rotate(-0.5deg);
        }
        22% {
            transform: translate(1.3px, 1.5px) rotate(-2deg);
        }
        24% {
            transform: translate(-1.4px, -1px) rotate(2deg);
        }
        26% {
            transform: translate(1.3px, -1.3px) rotate(0.5deg);
        }
        28% {
            transform: translate(1.6px, -1.6px) rotate(-1.5deg);
        }
        30% {
            transform: translate(-1.3px, -1.3px) rotate(-1.5deg);
        }
        32% {
            transform: translate(-1px, 0px) rotate(2deg);
        }
        34% {
            transform: translate(1.3px, 1.3px) rotate(-0.5deg);
        }
        36% {
            transform: translate(1.3px, 1.6px) rotate(1.5deg);
        }
        38% {
            transform: translate(1.3px, -1.6px) rotate(1.5deg);
        }
        40% {
            transform: translate(-1.4px, -1px) rotate(-0.5deg);
        }
        42% {
            transform: translate(-1.4px, 1.3px) rotate(-0.5deg);
        }
        44% {
            transform: translate(-1.6px, 1.4px) rotate(0.5deg);
        }
        46% {
            transform: translate(-2.1px, -1.3px) rotate(-0.5deg);
        }
        48% {
            transform: translate(1px, 1.6px) rotate(1.5deg);
        }
        50% {
            transform: translate(1.6px, 1.6px) rotate(1.5deg);
        }
        52% {
            transform: translate(-1.4px, 1.6px) rotate(0.5deg);
        }
        54% {
            transform: translate(1.6px, -1px) rotate(-2deg);
        }
        56% {
            transform: translate(1.3px, -1.6px) rotate(-2deg);
        }
        58% {
            transform: translate(-1.3px, -1.6px) rotate(0.5deg);
        }
        60% {
            transform: translate(1.3px, 1.6px) rotate(-0.5deg);
        }
        62% {
            transform: translate(0px, 0px) rotate(-1.5deg);
        }
        64% {
            transform: translate(-1.6px, -1.6px) rotate(-2deg);
        }
        66% {
            transform: translate(1.6px, -1.6px) rotate(0.5deg);
        }
        68% {
            transform: translate(0px, -1.6px) rotate(-2deg);
        }
        70% {
            transform: translate(-1.6px, 1px) rotate(1.5deg);
        }
        72% {
            transform: translate(-1.6px, 1.6px) rotate(2deg);
        }
        74% {
            transform: translate(1.3px, -1.6px) rotate(-0.5deg);
        }
        76% {
            transform: translate(1.4px, 1px) rotate(-0.5deg);
        }
        78% {
            transform: translate(-1px, 1.4px) rotate(2deg);
        }
        80% {
            transform: translate(1.4px, 1.6px) rotate(2deg);
        }
        82% {
            transform: translate(-1.6px, -1.6px) rotate(-0.5deg);
        }
        84% {
            transform: translate(-1.4px, 1.4px) rotate(-2deg);
        }
        86% {
            transform: translate(1px, 1.4px) rotate(-2deg);
        }
        88% {
            transform: translate(-1.4px, 1.4px) rotate(-1.5deg);
        }
        90% {
            transform: translate(-1.6px, -1.6px) rotate(-2deg);
        }
        92% {
            transform: translate(-1.4px, 1.6px) rotate(2deg);
        }
        94% {
            transform: translate(-1.6px, -1.6px) rotate(-2deg);
        }
        96% {
            transform: translate(-1.4px, 1.3px) rotate(-2deg);
        }
        98% {
            transform: translate(1.3px, 1px) rotate(-0.5deg);
        }
    }
</style>
