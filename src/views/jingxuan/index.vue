<style scoped lang='stylus'>
.content {
    background: #eee;
    width: 100%;
    .box {
        background: #fff;
        width: 100%;
        padding: 10px;
        /*去掉padding区域的背景*/
        background-origin: content-box;
        background-clip: content-box;
        .header {
            padding: 12px;
            .touxiang {
                width: 30px;
                height: 30px;
                border-radius: 9999px;
                img {
                    width: 100%;
                }
            }
            .nickname {
                height: 30px;
                line-height: 30px;
                padding-left: 20px;
                font-size: 1.5rem;
            }
            .jubao {
                font-size: 1.2rem;
                color: #ccc;
                border: 1px solid #ccc;
                border-radius: 4px;
                padding: 2px 3px;
                margin-top: 4px;
                margin-right: 10px;
            }
        }
        .title {
            font-size: 1.4rem;
            padding: 10px;
            line-height: 24px;
        }
        .main {
            img {
                width: 100%;
            }
        }
        .footer {
            padding: 8px 4px;
            font-size: 1.5rem;
            span {
                line-height: 20px;
                display: inline-block;
                float: left;
                margin-left: 5px;
            }
            i {
                float: left;
            }
            .like {
                margin-right: 30px;
                margin-left: 10px;
                i {
                    display: inline-block;
                    height: 20px;
                    width: 20px;
                    background: url(../../assets/26_03.png) 0 0 /100% 100% no-repeat;
                }
            }
            .dislike {
                i {
                    display: inline-block;
                    height: 20px;
                    width: 20px;
                    background: url(../../assets/26_05.png) 0 0 /100% 100% no-repeat;
                }
            }
            .recommend {
                margin-right: 10px;
                i {
                    display: inline-block;
                    height: 20px;
                    width: 20px;
                    background: url(../../assets/26_07.png) 0 0 /100% 100% no-repeat;
                }
            }
        }
    }
}

</style>
<template>
    <div v-height="50" style="overflow-y:scroll;">
        <vui-swiper :swiperUrls="swiperUrls"></vui-swiper>
        <div class="content">
            <div class="box" v-for="item in morenshuju">
                <div class="header ovh">
                    <div class="touxiang fl ovh">
                        <img :src="item.avatar" alt="">
                    </div>
                    <div class="nickname fl">{{item.user_name}}</div>
                    <div class="jubao fr">举报</div>
                </div>
                <div class="title" v-if="item.title">{{item.title}}</div>
                <div class="main" v-for="child in item.media_data">
                    <!-- <img :src="item.media_data[0].wifi_img_url" alt=""> -->
                    <gif v-if="child.format == 'GIF'" :child="child"></gif>
					<jpeg v-if="child.format == 'JPEG'" :child="child"></jpeg>
					<png v-if="child.format == 'PNG'" :child="child"></png>
                </div>
                <div class="footer ovh">
                    <div class="like fl ovh">
                        <i></i>
                        <span>{{item.like_start + item._incrs.like}}</span>
                    </div>
                    <div class="dislike fl ovh">
                        <i></i>
                        <span>{{item.dislike_start + item._incrs.dislike}}</span>
                    </div>
                    <div class="recommend fr ovh">
                        <i></i>
                        <span>{{item.comment_total}}</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import VuiSwiper from "../../components/swiper.vue"
import gif from "../../components/gif.vue"
import jpeg from "../../components/jpeg.vue"
import png from "../../components/png.vue"
export default {
    data() {
        return {
            swiperUrls: [{
                imgUrl: "http://bq-img.peco.uodoo.com/qiqu/img/buz/banner/eeda245f00be3a76aa94dae8c9bd9c97.jpg",
                title: "日报里的万年龙套终于正式出道！"
            }, {
                imgUrl: "http://bq-img.peco.uodoo.com/qiqu/img/buz/banner/df85d84f4a3a17cefde5aa439ce12199.jpg",
                title: "女子朋友圈刷到“男友和别人结婚”！跑到婚礼现场大闹，新郎被警方带走"
            }],
            morenshuju: []
        }
    },
    created() {
        var vm = this;
        this.$http.get("../../../static/data.json").then((res) => {
            vm.morenshuju = res.data.mapList
            console.log(vm.morenshuju);
        });

    },
    mounted() {

    },
    components: {
        VuiSwiper,
        gif,
        jpeg,
        png
    },
    directives: {
        height: {
            inserted(el, binding) {
                $(el).height($(window).height() - binding.value)
            }
        }
    }
}

</script>
