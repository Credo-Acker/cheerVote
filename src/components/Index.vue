<template>
    <div class="index">
        <div class="rulewrap">
            <div class="rule" v-on:click="showRule">
                活动规则：记得发看过了荆防颗粒局火热欧担惊受恐几十块的就看到放得开就大放大发的的发的热惹我惹我热人娃儿娃儿我惹我人文人文任务二为东西的发的发的规范的话语。
            </div>
            <div class="glide" v-on:click="showRule">
                <img src="../assets/glide.png">
            </div>
        </div>
        <div class="indexInfowrap">
            <div class="indexInfo" v-if="information.length">
                <ul>
                    <li v-for="(item, index) in information" class="li_indexInfo" v-bind:key="index">
                        <router-link :to="{path:'/viewDetail/'+item.classId}">
                            <img v-bind:src="item.firstImg" class="firstImg">
                            <span class="className">{{item.className}}</span>
                            <span class="slogan">{{item.slogan}}</span>
                        </router-link>
                    </li>
                </ul>
            </div>
        </div>
        <div class="footer_index">
            <router-link to="/index">
                <div class="shouye">
                    <img src="../assets/hshouye.png">
                </div>
            </router-link>
            <router-link to="/answer">
                <div class="dati">
                    <img src="../assets/dati.png">
                </div>
            </router-link>
            <router-link to="/info">
                <div class="xiaoxi">
                    <img src="../assets/xiaoxi.png">
                </div>
            </router-link>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Index',
    data () {
        return {
            //main index
            isRuleMove: false,
            information: [],
            api: "https://wx.yyeke.com/cheer_vote"
        }
    },
    created() {
        //获取首页信息
        this.$http.get(this.api+'/vote/user/cheer_info/firstPage')
            .then((response) => {
                this.information = response.data;
            })
            .catch((error) => {
                console.log(error);
            });
    },
    mounted() {

    },
    methods: {
        //规则函数
        showRule: function () {
            let rule = document.querySelector('.rule');
            if (this.isRuleMove) {
                rule.className = "rule ruleMoveDown";
                this.isRuleMove = !this.isRuleMove;
            } else {
                rule.className = "rule ruleMoveUp";
                this.isRuleMove = !this.isRuleMove;
            }
        }
    }
}
</script>

<style scoped>
li {
    list-style: none;
}

.index {
    width: 10rem;
    height: 100%;
    position: relative;
    top: 0;
}

/*规则*/
.rulewrap {
    position: relative;
    top: 0;
    width: 10rem;
    height: 0.6rem;
}

.rule {
    position: relative;
    padding: 0.16rem 0.4rem 0.16rem 0.4rem;
    width: 9.2rem;
    height: 1.6rem;
    font-size: 0.346rem;
    line-height: 0.55rem;
    cursor: pointer;
    overflow: hidden;
    z-index: 3;
    color: #ffffff;
    background: #50ceff;
}

.glide {
    position: relative;
    top: 0;
    margin: 0 auto;
    width: 0.573rem;
    height: 0.28rem;
    z-index: 3;
    background-color: #50ceff;
    border-bottom-left-radius: 0.1rem;
    border-bottom-right-radius: 0.1rem;
}

.glide img {
    position: absolute;
    left: 0.138rem;
    top: 0.02rem;
    width: 0.28rem;
    height: 0.186rem;
}

.ruleMoveUp {
    animation: ruleMoveUp 1s;
    animation-fill-mode: forwards;
}

.ruleMoveDown {
    animation: ruleMoveDown 1s;
    animation-fill-mode: forwards;
}

@keyframes ruleMoveUp {
    0% {
        height: 1.6rem;
    }
    100% {
        height: 0.45rem;
    }
}

@keyframes ruleMoveDown {
    0% {
        height: 0.45rem;
    }
    100% {
        height: 1.6rem;
    }
}


/*首页*/
.index {
    position: relative;
    display: inline-block;
    width: 10rem;
    height: 100%;
    background-color: #f5f5f5;
}

.indexInfowrap {
    position: relative;
    width: 10rem;
    height: 14rem;
    overflow: scroll;
}

.li_indexInfo {
    position: relative;
    margin: 0 auto;
    margin-top: 0.6rem;
    width: 9.146rem;
    height: 5.64rem;
    border-radius: 0.133rem;
    background-color: #ffffff;
}

.firstImg {
    display: block;
    position: absolute;
    top: 0.3rem;
    left: 0.373rem;
    width: 8.493rem;
    height: 3.773rem;
    border-radius: 0.133rem;
}

.className {
    position: absolute;
    top: 4.373rem;
    display: block;
    left: 0.373rem;
    width: 8.493rem;
    font-size: 0.386rem;
    color: #4e4e4e;
}

.slogan {
    position: absolute;
    display: block;
    top: 4.933rem;
    left: 0.373rem;
    width: 8.493rem;
    font-size: 0.333rem;
    color: #9f9f9f;
}

.footer_index {
    position: fixed;
    bottom: 0;
    width: 10rem;
    height: 1.33rem;
    font-size: 20px;
    border-top: 1px solid #dedede;
    background-color: #ffffff;
}

.shouye,.dati,.xiaoxi {
    cursor: pointer;
    width: 3.333rem;
    height: 1.33rem;
    float: left;
    background: #ffffff;
}

.shouye img,.dati img,.xiaoxi img {
    width: 0.586rem;
    height: 0.933rem;
    top: 0.213rem;
}

.shouye img {
    position: absolute;
    left: 0.96rem;
}

.dati img {
    position: relative;
    display: block;
    margin: 0 auto;
}

.xiaoxi img {
    position: absolute;
    right: 0.96rem;
}
</style>
