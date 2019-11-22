<template>
    <div class="index">
        <div class="text">
            <div class="centianer">
                <div class="title">打卡奖励规则</div>
                <div class="text-p">
                    <p>① 满28.5元就可以找老板扫码打卡</p>
                    <p>② 打卡7次赠送任意8元蛋糕</p>
                    <p>③ 打卡12次赠送6寸西点</p>
                    <p>④ 打卡22次赠送6寸生日蛋糕</p>
                    <p>⑤ 长按保存打卡二维码，下次就能直接使用手机相册里二维码图片打卡</p>
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
                <div class="left" @click="animationLeft=false;animationRight=true"></div>
                <div class="reward-center">
                    <div class="reward-content" :class="{'animation-left':animationLeft,'animation-right':animationRight}">
                        <div style="width: 50%;float: left">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount === 0" alt="" >
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount > 0" alt="" @click="reward(1)" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="info.signinCount < 2" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="info.signinCount >= 2" alt=""  class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 3" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 3" alt=""  @click="reward(1)" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="info.signinCount < 4" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="info.signinCount >= 4" alt="" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 5" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 5" alt="" @click="reward(1)" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="info.signinCount < 6" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="info.signinCount >= 6" alt="" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 7" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 7" alt="" @click="reward(7)" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="info.signinCount < 8" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="info.signinCount >= 8" alt="" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 9" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 9" alt="" @click="reward(1)" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="info.signinCount < 10" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="info.signinCount >= 10" alt="" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 11" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 11" alt=""  @click="reward(1)" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 12" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 12" alt="" @click="reward(12)" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="info.signinCount < 13" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="info.signinCount >= 13" alt="" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 14" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 14" alt="" @click="reward(1)"  class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="info.signinCount < 15" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="info.signinCount >= 15" alt="" class="shake">
                        </div>
                        <div style="width: 50%;float: left">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 16" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 16" alt="" @click="reward(1)" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="info.signinCount < 17" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="info.signinCount >= 17" alt="" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 18" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 18" alt="" @click="reward(1)" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="info.signinCount < 19" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="info.signinCount >= 19" alt="" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 20" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 20" alt="" @click="reward(1)" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_1.png" v-if="info.signinCount < 21" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_2.png" v-if="info.signinCount >= 21" alt="" class="shake">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_3.png" v-if="info.signinCount < 22" alt="">
                            <img src="https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/icon_4.png" v-if="info.signinCount >= 22" alt="" @click="reward(22)">
                        </div>
                    </div>
                </div>
                <div class="right" @click="animationLeft=true;animationRight=false"></div>
            </div>
            <div class="bottom">已打卡{{ info.signinCount }}次</div>
        </div>
        <transition name="fade">
            <div class="model" v-show="model">
                <div class="model-cotent">
                    <div class="model-cole" @click="model=false"></div>
                    <h3>打卡奖品</h3>
                    <p>奖品为： {{ rewardCent }} </p>
                </div>
            </div>
        </transition>
    </div>
</template>

<script>
    export default {
        name: 'index',
        data() {
            return {
                model: false,
                rewardCent: '',
                timeName: '',
                data: {},
                info: {},
                value: '',
                animationLeft: false,
                animationRight: false
            }
        },
        methods: {
            reward(item) {
                if (item === 1||item === 3||item === 5||item === 9||item === 11||item === 14||item === 16||item === 18||item === 20) {
                    this.rewardCent = '蛋挞一份'
                } else if (item === 7) {
                    this.rewardCent = '任选8元蛋糕一份'
                } else if (item === 12) {
                    this.rewardCent = '6寸西点一份'
                } else if (item === 22) {
                    this.rewardCent = '6寸生日蛋糕一份'
                }
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
                this.timeName = setInterval(() => {
                    this.$axios({
                        method: 'post',
                        url: "/appservice/wechat/qrcode/queryUserinfo?id="+this.data.data
                    }).then((item) => {
                        let data = item.data;
                        if (data.state === 200) {
                            this.info = data.data;
                            let loading = document.getElementById('Loading');
                            if (loading != null) {
                                document.body.removeChild(loading);
                            }
                        }
                    })
                },2000);
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
        background: url("https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/background.f9ae2d1.png") no-repeat top center;
        background-size: 100%;
        min-height: 100vh;
        padding-top: 18vw;
        overflow: hidden;
    }
    .text {
        width: 90%;
        margin: 0 auto;
        background: url("https://konkonyu.oss-cn-beijing.aliyuncs.com/qiandao/img/text-background.7889ec7.png") no-repeat top center;
        background-size: 100%;
        padding: 8px;
    }
    .text .centianer {
        background: #fff;
        border-top-left-radius: 30px;
        border-top-right-radius: 30px;
        height: 56vw;
        /*overflow-y: auto;*/
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
        width: 40%;
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
        padding-bottom: 5vw;
        overflow-y: hidden;
    }
    .center .left {
        float: left;
        width: 10%;
        height: 42vw;
        background: url("../assets/image/left.png") no-repeat center center;
        background-size: 40%;
    }

    .center .right {
        float: left;
        width: 10%;
        height: 42vw;
        background: url("../assets/image/rigth.png") no-repeat center center;
        background-size: 40%;
    }
    .center .reward-center {
        float: left;
        width: 80%;
        height: 42vw;
        overflow: hidden;
        position: relative;
    }
    .reward-content img {
        float: left;
        display: block;
        width: 20%;
    }
    .reward-content {
        position: absolute;
        top: 0;
        left: 0;
        width: 200%;

    }
    .animation-left {
        animation: mymove .5s linear;
        animation-fill-mode:forwards
    }
    @keyframes mymove
    {
        0%   {left:0}
        50%  {left:-50%}
        100% {left:-100%}
    }
    .animation-right {
        animation: mymove1 .5s linear;
        animation-fill-mode:forwards
    }
    @keyframes mymove1
    {
        0%   {left:-100%}
        50%  {left:-50%}
        100% {left:0}
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
    }
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
