<template>
    <div class="home">
        <div class="barrages-drop">
            <vue-baberrage
                    :isShow="barrageIsShow"
                    :barrageList="barrageList"
                    :maxWordCount="maxWordCount"
                    :throttleGap="throttleGap"
                    :loop="barrageLoop"
                    :boxHeight="boxHeight"
                    :messageHeight="messageHeight"
                    :message-gap="10"
            >
            </vue-baberrage>
        </div>
        <!--<div class="foot">京ICP备20013876<br>@laughing 2020 - 2020</div>-->
    </div>
</template>
<script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.js"></script>
<script>

    // @ is an alias to /src
    import Vue from 'vue';
    import $axios from 'axios'
    import {vueBaberrage, MESSAGE_TYPE} from 'vue-baberrage';

    Vue.use(vueBaberrage);

    export default {
        name: "Home",
        components: {},
        data() {
            return {
                timer: '',
                barrageIsShow: true,
                messageHeight: 40,
                boxHeight: 450,
                barrageLoop: false,
                maxWordCount: 20,
                throttleGap: 2000,
                barrageList: [],
                records: []

            }
        },
        mounted() {
            this.timer = setInterval(this.refush, 8000);
        },

        methods: {
            refush() {
                $axios({
                    url: "http://localhost/danmaku/show",
                    async: false,
                    method: "get",
                }).then(res => {
                    this.records = res.data.records;
                    console.log(this.records);
                    this.records.forEach((v) => {
                        this.barrageList.push({
                            id: v.id,
                            avatar: v.pic,
                            msg: v.msg,
                            time: this.getRandom(5,7),
                            type: MESSAGE_TYPE.NORMAL,
                            barrageStyle: 'red'
                        });

                    });

                })
            },

            getRandom(start, end, fixed) {
                fixed = 0;
                let differ = end - start
                let random = Math.random()
                return (start + differ * random).toFixed(fixed)
            }

        },
        beforeDestroy() {
            clearInterval(this.timer);
        }
    };
</script>
<style>
    body {
        background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url(https://api.dujin.org/bing/1920.php) 0px 0px no-repeat;
    }

    header {
        color: #ffffff;
        font-size: 30px;
        text-align: center;
        line-height: 290px;
    }

    .el-form-item__label {
        color: #ffffff;
    }


    .app-block {
        width: 28%;
        margin: 0 auto;
        text-align: center;
        padding: 3em 5em;
        height: 80%;
    }

    .foot {

        color: white;
        font-size: 14px;
        position: fixed;
        bottom: 8%;
        left: 45%;
    }
</style>
