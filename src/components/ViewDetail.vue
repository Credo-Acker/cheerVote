<template>
    <div class="viewDetail">
        <div class="top">
            <router-link to="/index">
                <img src="../assets/return.png" class="return">
            </router-link>
        </div>
        <div class="mainwrap" v-if="information">
            <div class="introduction">
                <wc-swiper :duration="1500" :interval="5000" v-if="slides.length">
                    <wc-slide v-for="(item,key) in slides" :key="key">
                        <img :src="item">
                    </wc-slide>
                </wc-swiper>
                <div class="className">
                    {{information.classname}}
                </div>
                <div class="slogan">
                    {{information.slogan}}
                </div>
                <div class="intro">
                    <p class="p_intro_title">
                         拉拉队简介：
                    </p>
                    <p class="p_intro">
                        {{information.introduce}}
                    </p>
                </div>
            </div>
            <div class="division">

            </div>
            <div class="video">
                <div class="video_title">
                    <img src="../assets/log.png"><span>拉拉队宣传视频</span>
                </div>
                <div class="video_main">
                    <div v-if="android == true">
                        <iframe :src="video" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="100%">  </iframe>
                    </div>
                    <div v-else-if="ios == true">
                        <video :src="video" controls preload width="100%" height="100%"></video>
                    </div>
                    <div v-else-if="android == false && ios == false">
                        <video :src="video" controls preload width="100%" height="100%"></video>
                    </div>
                    <!-- <div>
                        <video :src="video" controls preload width="100%" height="100%"></video>
                    </div> -->
                </div>
            </div>
            <div class="division">

            </div>
            <div class="picture">
                <div class="picture_title">
                    <img src="../assets/log.png"><span>拉拉队宣传图片</span>
                </div>
                <ul class="picture_main">
                    <li class="picture_item" v-for="(item, index) in duiyuan" @click="openImg(index)" :key="index">
                        <img :src="item.imgUrl">
                    </li>
                </ul>
            </div>
        </div>
        <div class="footer_view">
            <div class="cheer" @click="cheer">
                <img src="../assets/weizhuli.png">
            </div>
            <router-link to="/answer">
                <div class="answer">
                    <img src="../assets/weidati.png">
                </div>
            </router-link>
        </div>
        <div class="shadow no">
        </div>
        <img src="../assets/success.png" class="shadowimg no">
        <div class="bigImg opnone" @click="close">
            <wc-swiper :duration="1500" :autoplay="false" :defaultSlide="0" :pagination="false" ref="swiper" v-if="duiyuan.length">
                <wc-slide v-for="(item, key) in duiyuan" :key="key">
                    <img :src="item.imgUrl">
                    <div class="description">
                        <div class="description_title">
                            {{item.playername}}
                        </div>
                        <div class="description_cont">
                            {{item.introduce}}
                        </div>
                        <span>{{key + 1}}/{{duiyuanLength}}</span>
                    </div>
                </wc-slide>
            </wc-swiper>
        </div>
        <div class="cheerAlert no">
            <p>
                为拉拉队投票
            </p>
            <input type="text" class="cheer_to_num" placeholder="请输入投票数" onkeyup="this.value=this.value.replace(/\D/g,'')" onafterpaste="this.value=this.value.replace(/\D/g,'')">
            <span v-if="cheerNum >= 0">我的可用投票数：<i class="red">{{cheerNum}}</i></span>
            <div class="confirm" @click="send">

            </div>
        </div>
        <div class="cheerAlert2 no">
            <p>
                投票成功
            </p>
            <span v-if="cheerNum >= 0">我的可用投票数：<i class="red">{{cheerNum}}</i></span>
            <div class="confirm2" @click="closeAlert">

            </div>
        </div>
    </div>
</template>

<script>
import wcSwiper from '../assets/wc-swiper.js';
export default {
    name: 'ViewDetail',
    data() {
        return {
            classid: 0,
            groupid: 0,
            information: {},
            slides: [],
            currentSlide: 1,
            cheerNum: "",
            duiyuan: [],
            duiyuanLength: 0,
            video_List1: {
                "通信与信息工程学院": "tongxin.mp4",
                "计算机科学与技术学院": "jike.mp4",
                "自动化学院": "zidong.mp4",
                "先进制造工程学院": "xianjin.mp4",
                "光电工程学院/重庆国际半导体学院": "guangdian.mp4",
                "软件工程学院": "ruanjian.mp4",
                "生物信息学院": "shengwu.mp4",
                "理学院": "li.mp4",
                "经济管理学院/现代邮政学院": "jingguan.mp4",
                "传媒艺术学院": "chuanmei.mp4",
                "外国语学院": "waiguo.mp4",
                "国际学院": "guoji.mp4",
                "网络空间安全与信息法学院": "anfa.mp4"
            },
            video_List2: {
                "通信与信息工程学院": "//player.bilibili.com/player.html?aid=22267274&cid=36858442&page=1",
                "计算机科学与技术学院": "//player.bilibili.com/player.html?aid=22267086&cid=36858104&page=1",
                "自动化学院": "//player.bilibili.com/player.html?aid=22267369&cid=36858571&page=1",
                "先进制造工程学院": "//player.bilibili.com/player.html?aid=22267322&cid=36858537&page=1",
                "光电工程学院/重庆国际半导体学院": "//player.bilibili.com/player.html?aid=22267049&cid=36858009&page=1",
                "软件工程学院": "//player.bilibili.com/player.html?aid=22267209&cid=36858299&page=1",
                "生物信息学院": "//player.bilibili.com/player.html?aid=22267229&cid=36858358&page=1",
                "理学院": "//player.bilibili.com/player.html?aid=22267178&cid=36858258&page=1",
                "经济管理学院/现代邮政学院": "//player.bilibili.com/player.html?aid=22267158&cid=36858132&page=1",
                "传媒艺术学院": "//player.bilibili.com/player.html?aid=22178822&cid=36688616&page=1",
                "外国语学院": "//player.bilibili.com/player.html?aid=22267291&cid=36858511&page=1",
                "国际学院": "//player.bilibili.com/player.html?aid=22267073&cid=36858042&page=1",
                "网络空间安全与信息法学院": "//player.bilibili.com/player.html?aid=22267029&cid=36857898&page=1"
            },
            ios: false,
            android: false,
            video: "",
            api: "https://wx.idsbllp.cn/cheer_vote"
        }
    },
    created() {
        this.classid = this.$route.params.classid;
        switch (this.classid.toString()) {
            case "1": this.groupid = 2;
                    break;
            case "2": this.groupid = 4;
                    break;
            case "3": this.groupid = 4;
                    break;
            case "4": this.groupid = 2;
                    break;
            case "5": this.groupid = 1;
                    break;
            case "6": this.groupid = 3;
                    break;
            case "7": this.groupid = 2;
                    break;
            case "8": this.groupid = 2;
                    break;
            case "9": this.groupid = 3;
                    break;
            case "10": this.groupid = 1;
                    break;
            case "11": this.groupid = 4;
                    break;
            case "12": this.groupid = 3;
                    break;
            case "13": this.groupid = 1;
                    break;
        }
        //获取详情页数据
        this.$http.get(this.api+'/vote/user/cheer_info/main', {
                params: {
                    classId: this.$route.params.classid
                }
            })
            .then((response) => {
                this.information = response.data;
                this.slides.push(this.information.playImg1);
                this.slides.push(this.information.playImg2);
                this.slides.push(this.information.playImg3);
                this.isWeiXin();
                if (this.android) {
                    this.video = this.video_List2[response.data.classname];
                } else if (this.ios) {
                    this.video = "http://wx.yyeke.com/nodejs/cheerVote/video/" + this.video_List1[response.data.classname];
                } else {
                    this.video = "http://wx.yyeke.com/nodejs/cheerVote/video/" +  this.video_List1[this.information.classname];
                }
                // this.video = "http://wx.yyeke.com/nodejs/cheerVote/video/" +  this.video_List1[this.information.classname];
            })
            .catch((error) => {
                console.log(error);
            });
        //获取详情页拉拉队员
        this.$http.get(this.api+'/vote/user/cheerPlayerInfo', {
                params: {
                    classId: this.$route.params.classid
                }
            })
            .then((response) => {
                this.duiyuan = response.data;
                // for (let i = 0; i < this.duiyuan.length; i++) {
                //     if (!this.duiyuan[i].imgUrl) {
                //         this.duiyuan.splice(i, 1);
                //     }
                // }
                this.duiyuanLength = response.data.length;
            })
            .catch((error) => {
                console.log(error);
            });
        //获取票数
        this.$http.get(this.api+'/vote/user/assistance')
            .then((response) => {
                if (response.data.assistance != null) {
                    this.cheerNum = response.data.assistance;
                } else {
                    this.cheerNum = 0;
                }
            })
            .catch((error) => {
                console.log(error);
            });
    },
    mounted() {
    },
    methods: {
        openImg: function (index) {
            let bigImg = document.querySelector('.bigImg');
            this.currentSlide = index;
            this.$refs.swiper.slideTo(index);
            bigImg.className = 'bigImg';
        },
        close: function () {
            let bigImg = document.querySelector('.bigImg');
            bigImg.className = 'bigImg opnone';
        },
        cheer: function () {
            let cheerAlert = document.querySelector('.cheerAlert');
            let shadow = document.querySelector('.shadow');
            cheerAlert.className = "cheerAlert";
            shadow.className = "shadow";
        },
        send: function () {
            let cheerAlert = document.querySelector('.cheerAlert');
            let cheerToNum = document.querySelector('.cheer_to_num');
            let cheerAlert2 = document.querySelector('.cheerAlert2');
            let shadow = document.querySelector('.shadow');

            if (cheerToNum.value == "" || parseInt(cheerToNum.value) == 0) {
                alert("输入不合法");
                cheerToNum.value = "";
                cheerAlert.className = "cheerAlert no";
                shadow.className = "shadow no";
                return false;
            }
            if (cheerToNum.value > this.cheerNum) {
                alert("投票数超过您拥有的票数！");
                cheerToNum.value = "";
                cheerAlert.className = "cheerAlert no";
                shadow.className = "shadow no";
                return false;
            }

            let data = [];
            data.push({
                classId: this.classid,
                num: parseInt(cheerToNum.value),
                groupId: this.groupid
            });
            let string = this.$base64.encode(JSON.stringify(data));
            let timestamp = Math.round(new Date().getTime() / 1000).toString();
            let nonce = parseInt(100 * Math.random());
            let signature = this.$sha1(this.$md5(string + timestamp + nonce) + 'cheer_vote');
            let params = {
                "string": string,
                "timestamp": timestamp,
                "nonce": nonce,
                "signature": signature
            };
            //发送助力数
            this.$http.post(this.api+'/vote/user/cheer/distance', params)
                .then((response) => {
                    this.$http.get(this.api+'/vote/user/assistance')
                        .then((response) => {
                            this.cheerNum = response.data.assistance;
                            cheerAlert.className = "cheerAlert no";
                            cheerAlert2.className = "cheerAlert2";
                            cheerToNum.value = "";
                        })
                        .catch((error) => {
                            console.log(error);
                        });
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        closeAlert: function () {
            let cheerAlert2 = document.querySelector('.cheerAlert2');
            let shadow = document.querySelector('.shadow');

            cheerAlert2.className = "cheerAlert2 no";
            shadow.className = "shadow no";
        },
        isWeiXin: function () {
            let ua = window.navigator.userAgent.toLowerCase();

        	if (/iphone/i.test(ua)) {
                this.ios = true;
                this.android = false;
            }
            if (/android/i.test(ua)) {
                this.ios = false;
                this.android = true;
            }
        }
    }
}

</script>

<style scoped>
.viewDetail {
    position: relative;
    display: inline-block;
    left: 0;
    top: 0;
    width: 10rem;
    height: 100%;
}

/*顶部*/
.top {
    position: relative;
    width: 10rem;
    top: 0;
    height: 1rem;
}

.top a {
    position: relative;
    left: 0.42rem;
    top: 0.293rem;
    width: 0.48rem;
    height: 0.43rem;
}

.return {
    position: relative;
    width: 0.48rem;
    height: 0.426rem;
}

/*主体部分*/
.mainwrap {
    position: relative;
    width: 10rem;
    height: 14rem;
    overflow: scroll;
}

.introduction {
    position: relative;
    left: 0.42rem;
    width: 9.16rem;
    margin-bottom: 0.3rem;
}

.wc-slide img {
    width: 9.16rem;
    height: 4.04rem;
}

.className {
    position: relative;
    margin-top: 0.35rem;
    margin-bottom: 0.1rem;
    color: #444444;
    font-size: 0.373rem;
}

.slogan {
    color: #848484;
    font-size: 0.32rem;
}

.intro {
    margin-top: 0.33rem;
    position: relative;
}

.p_intro_title {
    font-size: 0.346rem;
    color: #8a8a8a;
}

.p_intro {
    line-height: 0.54rem;
    color: #9f9f9f;
    font-size: 0.32rem;
}

.division {
    height: 0.2rem;
    background: #f5f5f5;
}

.video_title {
    position: relative;
    padding-left: 0.42rem;
    font-size: 0.373rem;
    color: #031921;
    line-height: 0.453rem;
    vertical-align: middle;
}

.video_title img {
    display: inline-block;
    margin-right: 0.266rem;
    height: 0.453rem;
    width: 0.066rem;
    margin-top: 0.4rem;
    border-radius: 0.133rem;
}

.video_title span {
    position: relative;
    display: inline-block;
    top: -0.1rem;
}

.video {
    padding-bottom: 0.42rem;
}

.video_main {
    position: relative;
    margin: 0 auto;
    width: 9.16rem;
    height: 4.04rem;
    margin-top: 0.33rem;
    border-radius: 0.2rem;
}

.picture {
    position: relative;
}

.picture_title {
    position: relative;
    padding-left: 0.42rem;
    font-size: 0.373rem;
    color: #031921;
    line-height: 0.453rem;
    vertical-align: middle;
}

.picture_title img {
    display: inline-block;
    margin-right: 0.266rem;
    height: 0.453rem;
    width: 0.066rem;
    margin-top: 0.4rem;
}

.picture_title span {
    position: relative;
    display: inline-block;
    top: -0.1rem;
}

.picture_main {
    display: inline-block;
    position: relative;
    width: 9.21rem;
    left: 0.42rem;
    margin-top: 0.33rem;
}

.picture_item {
    display: inline-block;
    float: left;
    width: 3.01rem;
    height: 3.01rem;
    margin-right: 0.05rem;
    margin-bottom: 0.05rem;
}

.picture_item img {
    width: 3.01rem;
    height: 3.01rem;
    border-radius: 3px;
}

/*底部*/
.footer_view {
    position: fixed;
    bottom: 0rem;
    width: 10rem;
    z-index: 10;
    height: 1.133rem;
    font-size: 20px;
    border-top: 1px solid #dedede;
}

.footer_view a {
    display: inline-block;
    top: -1.17rem;
    width: 4.986rem;
    float: left;
}

.cheer,.answer {
    display: inline-block;
    width: 4.986rem;
    height: 1.133rem;
    float: left;
}

.cheer img,.answer img {
    width: 4.986rem;
    height: 1.133rem;
}

.shadow {
    position: absolute;
    top: 0;
    left: 0;
    width: 10rem;
    height: 100%;
    z-index: 98;
    background: #000000;
    opacity: 0.8;
}

.cheerAlert {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 7.426rem;
    height: 8.4rem;
    margin-left: -3.713rem;
    margin-top: -4.2rem;
    opacity: 1;
    z-index: 99;
    background: url(../assets/bg_alert.png);
    background-size: 100% 100%;
}

.cheerAlert p {
    position: relative;
    left: 0.733rem;
    top: 4rem;
    font-size: 0.373rem;
    color: #5b5b5b;
}

.cheerAlert input {
    position: relative;
    top: 4.4rem;
    left: 0.733rem;
    line-height: 0.8rem;
    font-size: 0.293rem;
    width: 5.56rem;
    padding: 0 0.2rem;
    border: 0.013rem solid #c2c2c2;
    border-radius: 0.1rem;
    color: #b4b4b4;
    -webkit-appearance: none;
}

:-moz-placeholder { /* Mozilla Firefox 4 to 18 */
    color: #c4c4c4;
    opacity:1;
}

::-moz-placeholder { /* Mozilla Firefox 19+ */
    color: #c4c4c4;
    opacity:1;
}

input:-ms-input-placeholder{
    color: #c4c4c4;
    opacity:1;
}

input::-webkit-input-placeholder{
    color: #c4c4c4;
    opacity:1;
}

.cheerAlert span {
    position: relative;
    display: inline-block;
    color: #9e9e9e;
    font-size: 0.32rem;
    top: 4.6rem;
    left: 0.733rem;
}

.red {
    color: #ff8080;
    font-style:normal;
}

.cheerAlert .confirm {
    position: relative;
    left: 0.733rem;
    top: 5rem;
    width: 5.946rem;
    height: 0.906rem;
    background: url(../assets/confirm.png);
    background-size: 100% 100%;
}

.cheerAlert2 {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 7.426rem;
    height: 8.4rem;
    margin-left: -3.713rem;
    margin-top: -4.2rem;
    opacity: 1;
    z-index: 99;
    background: url(../assets/bg_alert2.png);
    background-size: 100% 100%;
}

.cheerAlert2 p {
    position: relative;
    display: inline-block;
    left: 2.73rem;
    top: 4.2rem;
    font-size: 0.453rem;
    color: #5b5b5b;
}

.cheerAlert2 span {
    position: relative;
    display: block;
    color: #9e9e9e;
    text-align: center;
    font-size: 0.32rem;
    top: 4.7rem;
}

.cheerAlert2 .confirm2 {
    position: relative;
    left: 0.733rem;
    top: 5.7rem;
    width: 5.946rem;
    height: 0.906rem;
    background: url(../assets/confirm.png);
    background-size: 100% 100%;
}

input:focus {
    outline: none;
}

.shadowimg {
    position: absolute;
    top: 50%;
    top: 50%;
    margin-top: -2.206rem;
    margin-left: -2.14rem;
    width: 4.28rem;
    height: 4.413rem;
    opacity: 1;
    z-index: 99;
}

.bigImg {
    position: absolute;
    top: 0;
    width: 10rem;
    background: rgba(0,0,0,0.85);
    height: 100%;
    z-index: 99;
}

.bigImg .wc-swiper-container {
    height: 100%;
}

.bigImg .wc-slide {
    width: 10rem;
    height: 100%;
    position: relative;
    display:-webkit-box;/* android 2.1-3.0, ios 3.2-4.3 */
    display:-webkit-flex;/* Chrome 21+ */
    display:-ms-flexbox;/* WP IE 10 */
    display:flex;/* android 4.4 */
    -webkit-box-align: center;/* android 2.1-3.0, ios 3.2-4.3 */
    -webkit-align-items: center;/* Chrome 21+ */
    -ms-flex-align: center;/* WP IE 10 */
    align-items: center;/* android 4.4 */
}

.bigImg .wc-slide img {
    /* top: 50%; */
    /* margin-top: -4.233rem; */
    /* height: 6.76rem; */
    width: 10rem;
    height: auto;
}

.description {
    position: absolute;
    bottom: 0;
    line-height: 0.6rem;
    width: 9.2rem;
    padding: 0.7rem 0.4rem;
    font-size: 0.346rem;
    color: #ffffff;
    background: #000000;
}

.description_title {
    position: relative;
    display: inline-block;
    color: #ffffff;
    opacity: 0.9;
    line-height: 0.7rem;
}

.description_cont {
    position: relative;
    display: inline-block;
    line-height: 0.58rem;
    width: 9.2rem;
    margin: 0 auto;
    word-break: break-all;
    word-wrap:break-word;
}

.description span {
    display: block;
    position: absolute;
    width: 10rem;
    bottom: 0.13rem;
    left: 0rem;
    text-align: center;
    font-size: 0.32rem;
    margin: 0 auto;
}

.opnone {
    visibility: hidden;
}

.no {
    display: none;
}
</style>
