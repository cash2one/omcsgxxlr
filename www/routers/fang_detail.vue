<style scoped lang="less">
  @import "../resources/css/website/order.less";
  @import "../resources/plugin/swiper/css/swiper.css"; /*swiper 轮播*/
  .weixin_wrap{padding-bottom:.2rem}
  .weixin_head span{width:47% !important;margin-right: 0.2rem !important}
  .weixin_head span.row{width:100% !important}
  .weixin_head span:not(.row){width:47% !important}
  .weixin_head{padding-bottom:.3rem;border-bottom:2px solid #e5e5e6}
  .pageState:before{margin-right: 4px;border-top: 6px solid transparent;border-bottom: 6px solid transparent;border-right: 6px solid #FFF}
  .pageState{position: absolute;right: 15px;bottom: 4px;padding: 2px 8px;line-height: 20px;font-size: 12px;color: #FFF; z-index: 3}
  .section{padding-bottom:1.2rem;padding-top:0 !important}
  .gift{padding-top:.3rem;width:100% !important}
  .weixin_bot .weixin_bot_box span{font-size:.27rem !important}
  .photoedit{position:absolute;width:1.4rem;bottom:.18rem;right:.2rem;border: none;background-color: white;color: #0c0c0c;z-index: 999;border-radius: .2rem;}
</style>
<template>
  <div>
    <!--context-->
    <section id="section" class="pr section">
      <div class="detail-container">
        <!--banner-->
        <div id="slideBox" class="slideBox">
          <div class="swiper-container">
            <div class="swiper-wrapper">
              <div class="swiper-slide" v-for="image in house_image">
                <a href="javascript:;" @click="enlarge">
                  <img v-if="image" :src="$prefix + '/' + image" alt="">
                  <img v-else :src="$prefix + '/upload/2017-08-27/6404b4de960b81fc5403c870aefcea34.png'" alt="">
                </a>

              </div>
            </div>
            <div class="banner-page">
              <span class="pageState"><span id="picIndex">1</span>/{{house_image.length}}</span>
              <span><mt-button type="primary" class="photoedit" @click.stop.prevent="upload">上传图片</mt-button></span>
            </div>
          </div>
        </div>
      </div>
      <!--点击出现放大图片-->
      <div v-show="large" class="large">
        <div class="clear" @click="cancel">x</div>
        <div style="clear: both;"></div>
        <div class="swiper-container" style="position:relative !important;top:26.5%!important;">
          <div class="swiper-wrapper">
            <div class="swiper-slide img" v-for="image in house_image">
              <a href="javascript:;">
                <img v-if="image" :src="$prefix + '/' + image" alt="">
                <img v-else :src="$prefix + '/upload/2017-08-27/6404b4de960b81fc5403c870aefcea34.png'" alt="">
              </a>
            </div>
          </div>
        </div>
      </div>
      <div class="build_price_wrap clearfix">
       <!-- <span><i v-text="zdh" style="color:black"></i> - <i v-text="fybh" style="color:black"></i></span>-->
        <span v-if="zdh.indexOf('独栋') > -1"><i v-text="fybh" style="color:black"></i></span>
        <span v-else><i v-text="zdh" style="color:black"></i> - <i v-text="fybh" style="color:black"></i></span>
        <span><i v-text="monthly_price" style="color:#e01222"></i><i v-if="monthly_price != null" style="color:black">元/月</i></span>
        <span v-text="daily_price" style="color:#e01222"></span><i v-if="daily_price != null">元/㎡/天</i>
      </div>

      <div class="build_common_msg_wrap">
        <a href="javascript:;"><span style="color:black">面积</span><i v-text="room_area" style="color:#5b5b5b;"></i></a>
        <a href="javascript:;"><span style="color:black">工位</span><i v-text="workstation" style="color:#5b5b5b;"></i></a>
        <a href="javascript:;"><span style="color:black">房间状态</span><i v-text="fjzt" style="color:#5b5b5b;"></i></a>
        <span class="common_ver_line"></span>
        <span class="common_ver_line second"></span>
      </div>

      <div class="weixin_wrap">
        <div class="weixin_head clearfix">
          <span>楼&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;层：<i v-text="locat_floor" style="color: #5b5b5b;"></i></i></span>
          <span>可否注册：<i v-text="zc" style="color: #5b5b5b;"></i></span>
          <span>层&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;高：<i v-text="fjcg" style="color: #5b5b5b;"></i></span>
          <span>朝&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;向：<i v-text="chx" style="color: #5b5b5b;">{{chx}}</i></span>
          <span>建成年代：<i v-text="kprq" style="color: #5b5b5b;"></i></span>
          <span>停&nbsp;&nbsp;车&nbsp;&nbsp;费：<i v-text="tcf" style="color: #5b5b5b;"></i></span>
          <span class="row">产权性质：<i v-text="chqxz" style="color: #5b5b5b;"></i></span>
          <span class="row">物业公司：<i v-text="wygs" style="color: #5b5b5b;"></i></span>
          <span>物&nbsp;&nbsp;业&nbsp;&nbsp;费：<i v-text="wyf" style="color: #5b5b5b;"></i></span>
          <span>供&nbsp;&nbsp;暖&nbsp;&nbsp;费：<i v-text="gnf" style="color: #5b5b5b;"></i></span>
          <span class="row">网络公司：<i v-text="wlgs" style="color: #5b5b5b;"></i></span>
        </div>
        <div class="weixin_head gift clearfix">
          <span class="row">礼品等级：<i v-text="lpdj"></i></i></span>
        </div>
        <div class="weixin_bot clearfix">
          <div class="fl weixin_bot_box">
            <span>销售顾问：<i v-text="name"></i></span>
            <span>联系方式：<a :href="'tel:' + phone"><i class="con_telephone" v-text="phone"></i></a></span>
          </div>
        </div>
      </div>
      <div class="tel-order clearfix">
        <a id="semwaploupanxiangqingdibu400" :href="'tel:' + phone" class="phone--tel-order">
          <img src="../resources/images/icons/phone-icon.png" class="mr05 mt-3">一键拨号</a>
      </div>

    </section>

    <!--context end-->

  </div>
</template>

<script>
  import header1 from '../components/header.vue';
  import footer1 from '../components/footer.vue';
  import {Indicator} from 'mint-ui';
  import {Toast} from 'mint-ui';
  import { Button } from 'mint-ui';
  import {InfiniteScroll} from 'mint-ui';
  import '../resources/plugin/swiper/js/swiper.min.js';

  export default {
    components: {header1, footer1},
    data () {
      return {
        large:false,
        hourse_id: "",
        daily_price:0, //日价格
        monthly_price:0, //月价格
        room_area:0, //面积
        workstation:0, //工位
        floors:0, //总楼层
        locat_floor:0, //所在楼层
        fjzt: "", 

        house_image: [],
        name: "",
        phone: "",
        fjcg: "",
        chx: "",
        wygs: '',//物业公司
          zdh:'',
        fybh:"",
        wyf: '',//物业费
        kprq: '',//建成年代
        tcf: "",
        wlgs: "",
        gnf: "",
        zc: "",
        chqxz: "",
        lpdj: "",
        building_images: [],
        property: {"1":"写字楼", "2":"公寓","3":"商务楼","4":"住宅","5":"商业","6":"酒店","7":"综合","8":"别墅","9":"商业综合体","10":"酒店式公寓"},

      }
    },
    methods: {
      //获取某一办公楼详情
      getPerDetail(){
        var _this = this;
        const house_id = this.$route.query.house_id;
        this.hourse_id = house_id;
        this.$http.post(
          this.$api + "/yhcms/web/lpjbxx/getWxFyxx.do",
          {
            "parameters": {
              "hourse_id": house_id
            },
            "foreEndType": 2,
            "code": "30000004"
          }
        ).then(function (res) {
          var result = JSON.parse(res.bodyText);
          Indicator.close();
          if (result.success) {
            if (result.data) {
              const data = result.data[0];
              $('title').html(result.data.name);
              _this.daily_price = !data.dj ? '暂无数据' : data.dj;
              _this.monthly_price = !data.yzj ? '暂无数据' : data.yzj;
              _this.room_area = !data.fjmj ? '暂无数据' : data.fjmj + '㎡';
              _this.workstation = data.krgw || '暂无数据';
              _this.floors = data.zglc || '暂无数据';
              _this.locat_floor = data.lc || '暂无数据';
             /* _this.wyf = !data.wyf ? '暂无数据' : data.wyf + '元/㎡/月';*/
              if(result.data.wyf == 0){
                  _this.wyf = '已包含';
              }else{
                  _this.wyf = !result.data.wyf ? '暂无数据' : result.data.wyf + '元/㎡/月'; //物业费
              }
                _this.zdh = data.zdh || '暂无数据';
              _this.fybh = data.fybh || '暂无数据';
              _this.wygs = data.wygs || '暂无数据';
              /*_this.fjcg = data.fjcg + "m" || '暂无数据';*/
              _this.fjcg = !data.fjcg ? '暂无数据' : data.fjcg + "m";
              _this.fjzt = data.fjzt || '暂无数据';
              _this.name = data.name || '暂无数据';
              _this.phone = data.phone || '暂无数据';
              _this.house_image = data.housing_icon.split(";");

              const zc = data.zc || '暂无数据';
              _this.zc = zc === '0' ? '不可注册' : '可注册';
              _this.chx = data.chx || '暂无数据';
              _this.kprq = data.kprq || '暂无数据';
              _this.chqxz = data.chqxz || '暂无数据';
              /*_this.gnf= !data.gnf ? '暂无数据' : data.gnf + '元/㎡/季';*/
              if(result.data.gnf == 0){
                  _this.gnf = '已包含';
              }else{
                  _this.gnf = !result.data.gnf ? '暂无数据' : result.data.gnf + '元/㎡/季';
              }
              _this.tcf = !data.tcf ? '暂无数据' : data.tcf + '元/月';
              _this.wlgs = data.wlgs || '暂无数据';
              _this.lpdj = data.lpsort || '暂无数据';
            }

            setTimeout(function(){
              var mySwiper = new Swiper('.swiper-container', {
                loop: true,
                paginationClickable: true,
                centeredSlides: true,
                autoplay: 3500,
                onSlideChangeStart: function (swiper) {
                    $("#picIndex").text(swiper.realIndex + 1);
                },
                autoplayDisableOnInteraction: true
              });
            }, 1000);
          }

        }, function (res) {
          Toast({
            message: '抱歉,获取楼盘详情失败!',
            position: 'middle',
            duration: 3000
          });
        });
      },
      cancel(){
          this.large = false;
      },
      enlarge(){
        this.large = true;
      },
      upload(){
          const that = this;
          setTimeout(function(){
              that.$router.push({path:'/fang_image/' + that.hourse_id});
          },300);
      }
    },
    mounted(){
      Indicator.open({
        text: '',
        spinnerType: 'fading-circle'
      });
      this.getPerDetail();
    }
  }
</script>
