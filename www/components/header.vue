<style lang="less">
  @import "../resources/css/reset.less";
  @import "../resources/css/color.less";
  @import "../resources/css/base.less";
  @import "../resources/css/website/list.less";

  [header]{
    .mint-popup{
      width:100%;
      position: fixed;
      left:0;
      top:0;
    }
  }

</style>
<template>
  <div header>
    <!--header start-->
    <header id="header" class="clearfix">
      <label class="side_nav side-nav" @click.native="popupVisible= true"  v-on:click="showMenu">
        <img src="../resources/images/ys_more.png" width="20" alt="">
      </label>
    <!--   <a href="javascript:void(0);" class="detail-search">
          <input type="text" id="keyword" placeholder="请输入关键字搜索" value="" maxlength="50">
      </a>
      <i class="location_icon"></i>
    </header>-->
    <!--<a href="javascript:;" class="detail-search" style="position: fixed;left: 0; top: 0">
      <input type="text" id="keyword" placeholder="请输入关键字搜索" v-model="para.search_keywork" maxlength="50"
             @focus="changeRou">
    </a>-->
      <a href="javascript:;" class="detail-search" style="position: fixed;left: 0; top: 0">
        <input type="text" id="keyword" placeholder="请输入关键字搜索" v-model="para.search_keywork" maxlength="50"
               @focus="changeRou">
      </a>
    <router-view></router-view>
    </header>
    <mt-popup v-model="popupVisible" position="left" class="mint-popup-3" :modal="false">
      <!--左侧登录div-->
      <div class="main-nav-wrapper sidenav">
        <div class="user-box clearfix">
          <img class="portrait" src="../resources/images/user/por_icon.png" alt="">
          <div class="user_name tc mb20">{{username}}</div>
          <div class="ys_function tc">
            <!--<router-link :to="{path:'/list2'}" id="first_list_link">房源列表</router-link>
            <a href="javascript:;">房源信息采集</a>
            <a href="javascript:;">待办任务</a>-->
            <a href="javascript:;" @click="loupan_zidian">楼盘字典</a>

            <!--跟进工单-->
            <!--<a href="javascript:;" @click="yixing_genjin">意向跟进</a>-->

            <a href="javascript:;" @click="modify_pwd">修改密码</a>
          </div>
        </div>
        <a href="javascript:;" class="log_out_btn" @click="login_out()">退出登录</a>
      </div>
    </mt-popup>
  </div>
  <!--header end-->
</template>
<script type="text/babel">
  import $ from 'jquery';
  import {Toast} from 'mint-ui';
  import {Indicator} from 'mint-ui';

  export default {
      components: {
          Toast,
      },
    data() {
      return {
        popupVisible: false,
          username:"",
          para: {
              "search_keywork": "",
          },
      };
    },
    methods: {
      init(){
          this.para.search_keywork = this.$route.query.keyword;
      },
      changeRou: function () {
          this.$router.push({path: '/search?rt=index'})
      },
      showMenu() {
        this.popupVisible = true;
        var wwd = $("#section").width();
        if (!wwd) {
          wwd = $(".section").width();
        }
        var wgd = $(window).height() + 50;
        $("body").prepend('<div id="zhezhao"></div>');
        $("#zhezhao").css({
          width: "" + wwd + "px",
          height: "" + wgd + "px",
          "background-color": "#000",
          "z-index": "12",
          opacity: "0.5",
          position: "absolute",
          top: "0px",
          left: "0px"
        });

        $(".sidenav").css("left", "-100%");
        $(".sidenav").show();
        $("#zhezhao").animate({
          left: "75%"
        }, 150);
        $("#zhezhao").animate({
          backgroundColor: "#000000"
        }, 150);

        $(".sidenav").css({
          height:$(window).height()
        });

        $(".sidenav").animate({
          left: "0"
        }, 150, function () {
          $('html').css({'height': '100%', 'overflow': 'hidden'});
          $('body').css({'height': '100%', 'overflow': 'hidden'});
          $("#zhezhao").on("click",
            function () {
              $('.sidenav').animate({
                left: '-100%'
              }, 150, function () {
                $('.sidenav').hide();
              });
              $("#section").animate({
                left: '0'
              }, 150);
              $(".section").animate({
                left: '0'
              }, 150);
              this.popupVisible = false;
              $("#zhezhao").remove();
              /*$('html').css({'height': 'auto', 'overflow': 'auto'});
              $('body').css({'height': 'auto', 'overflow': 'auto'});*/
              $('html').removeAttr("style");
              $("body").removeAttr("style");
            }
          )
        });
        $("#section").animate({
          left: "75%"
        }, 150);
        $(".section").animate({
          left: "75%"
        }, 150);

      },
      loupan_zidian(){
          if(window.location.href.indexOf("index") != -1 || window.location.href.lastIndexOf("/") != -1){
              $('.sidenav').animate({
                  left: '-100%'
              }, 150, function () {
                  $('.sidenav').hide();
              });
              $("#section").animate({
                  left: '0'
              }, 150);
              $(".section").animate({
                  left: '0'
              }, 150);
              this.popupVisible = false;
          }else{

          }
          $("#zhezhao").remove();
          $('html').removeAttr("style");
          $("body").removeAttr("style");
          this.$router.push({path:'/'});
      },
      yixing_genjin(){
          if(window.location.href.indexOf("yixing_genjin") != -1){
              $('.sidenav').animate({
                  left: '-100%'
              }, 150, function () {
                  $('.sidenav').hide();
              });
              $("#section").animate({
                  left: '0'
              }, 150);
              $(".section").animate({
                  left: '0'
              }, 150);
              this.popupVisible = false;
          }else{

          }
          $("#zhezhao").remove();
          $('html').removeAttr("style");
          $("body").removeAttr("style");
          this.$router.push({path:'/yixing_genjin'});
      },
      modify_pwd(){
          $("#zhezhao").remove();
          $('html').removeAttr("style");
          $("body").removeAttr("style");
          this.$router.push({path:'/modify_pwd'});
      },
      login_out(){
        $("#zhezhao").remove();
          const url = this.$api + "/yhcms/web/wxqx/getSgUser.do";
          const user22 = JSON.parse(localStorage.getItem('cook'));
          let that = this;
          this.$http.post(url,{ "cookie":user22.sjs,"foreEndType":2,"code":"300000086"}).then((res)=>{
              Indicator.close()
              const data = JSON.parse(res.bodyText).success;
              if(data){
                  $('html').removeAttr("style");
                  $("body").removeAttr("style");
                  //$('html').css({'height': 'auto', 'overflow': 'auto'});
                  localStorage.removeItem('cook');
                  this.$router.push({path:'/login'});
              }else{
                  Toast({
                      message: '系统异常，请稍后再试!',
                      position: 'middle',
                      duration: 3000
                  });
              }
          }, (res)=>{
              Indicator.close()
          });
      }
    },
    mounted: function () {
      this.init();
      var _this = this;
      let user = JSON.parse(localStorage.getItem('user'));
      this.username = user;
      $('#first_list_link').click(function(){
        $("#zhezhao").remove();
      });
    }
  };
</script>
