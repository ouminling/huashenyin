<template style="background:#f5f6fa;" xmlns:el-col="http://www.w3.org/1999/html">
  <div class="dashboard-editor-container">

    <!--走马灯-->
    <el-carousel type="card" height="250px">
      <el-carousel-item v-for="(img,index) in imgList" :key="index">
        <img :src="img.url" width="100%" height="100%" >
      </el-carousel-item>
    </el-carousel>

    <el-row :gutter="[24,24]" class="module-list">
      <!-- 项目介绍 -->
      <el-col :span="24" >
        <div class="module-in module-in01 modlule-show" style="height: 260px;">
          <p>
            <el-alert title="免费开源【欢迎star和fork项目,请点击下方按钮访问】" type="info" :closable="false" show-icon  effect="dark"/>
          </p>
          <p class="text-indent">
            基于若依-ruoyi-vue项目扩展，前端采用Ant-Design-VUE，代码易读易懂、界面简洁美观，不仅仅是一个后台开发框架，它是一个企业级快速开发解决方案，我们将把UI交互、快速开发能力追求到极致，适配国产数据库，国产中间件，将支持多租户、flowable工作流，移动APP，更多插件正在扩展中
          </p>


          <p>
            <el-row :gutter="[24,24]">
              <el-col :span="8">
                <el-button block style="height: 40px;" type="primary" @click="goTarget('https://gitee.com/huashenyin/huashenyin-admin')">
                  访问AiDex
                </el-button>
              </el-col>
              <el-col :span="8">
                <el-button block style="height: 40px;" type="primary" ghost @click="goTarget('https://www.cnblogs.com/huashenyin/')">
                  我的博客
                </el-button>
              </el-col>
              <el-col :span="8">
                <el-button block style="height: 40px;" type="primary" ghost @click="goTarget('https://gitee.com/y_project/RuoYi-Vue')">
                  访问若依
                </el-button>
              </el-col>
            </el-row>
          </p>
        </div>
      </el-col>
    </el-row>

    <el-row>
      <el-col :span="8">
        <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>统计信息</span>


            <div style="float: right">
            <el-date-picker
              v-model="date_value"
              type="datetime"
              placeholder="选择日期时间">
            </el-date-picker>
        </div>

        </div>
        <div class="text item">
          <!--环形进度条-->
          <el-progress type="circle" :percentage="0"></el-progress>
          <el-progress type="circle" :percentage="25"></el-progress>
          <el-progress type="circle" :percentage="100" status="success"></el-progress>
          <el-progress type="circle" :percentage="70" status="warning"></el-progress>
          <el-progress type="circle" :percentage="50" status="exception"></el-progress>
        </div>
      </el-card></el-col>

      <el-col :span="8">
        <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>技术选型</span>
        </div>
        <div v-for="o in technology_item" :key="o" class="text item">
          {{ o }}
        </div>
      </el-card>
      </el-col>

      <el-col :span="8">
        <!--评分-->
        <el-rate
          v-model="rate_value"
          show-text>
        </el-rate>
      </el-col>

    </el-row>
    <!--footer-->
    <el-footer>
      <el-row :gutter="20">
        <el-col :span="16">
          <el-col :span="4">
            <div class="pls-footer-website-group"><h3 class="pls-footer-website-title"><a>热门推荐</a></h3><ul class="pls-footer-website-list"><li><a href="https://cloud.tencent.com/product/cvm" hotrep="hp.footer.navigation.tuijian.cvm">云服务器</a></li><li><a href="https://cloud.tencent.com/product/lighthouse" hotrep="hp.footer.navigation.tuijian.lighthouse">轻量云服务器</a></li><li><a href="https://cloud.tencent.com/product/cos" hotrep="hp.footer.navigation.tuijian.cos">对象存储</a></li><li><a href="https://cloud.tencent.com/product/cdb" hotrep="hp.footer.navigation.tuijian.cdb">云数据库 MySQL</a></li><li><a href="https://dnspod.cloud.tencent.com/" hotrep="hp.footer.navigation.tuijian.dnspod">域名注册</a></li><li><a href="https://cloud.tencent.com/product/facerecognition" hotrep="hp.footer.navigation.tuijian.facerecognition">人脸识别</a></li><li><a href="https://cloud.tencent.com/product/trtc" hotrep="hp.footer.navigation.tuijian.trtc">实时音视频</a></li><li><a href="https://cloud.tencent.com/product/im" hotrep="hp.footer.navigation.tuijian.im">即时通信 IM</a></li></ul></div>

          </el-col>
          <el-col :span="4">
            <div class="pls-footer-website-group"><h3 class="pls-footer-website-title"><a>资源与社区</a></h3><ul class="pls-footer-website-list"><li><a href="https://cloud.tencent.com/document/product" hotrep="hp.footer.navigation.resource.document">产品文档</a></li><li><a href="https://cloud.tencent.com/document/api" hotrep="hp.footer.navigation.resource.api">API 中心</a></li><li><a href="https://cloud.tencent.com/document/sdk" hotrep="hp.footer.navigation.resource.sdk">SDK 中心</a></li><li><a href="https://cloud.tencent.com/guide" hotrep="hp.footer.navigation.resource.guide">入门中心</a></li><li><a href="https://app.cloud.tencent.com/" hotrep="hp.footer.navigation.resource.app">开源应用中心</a></li><li><a href="https://cloud.tencent.com/developer" hotrep="hp.footer.navigation.resource.developer">云+社区</a></li><li><a href="https://cloud.tencent.com/edu/training" hotrep="hp.footer.navigation.resource.training">培训认证</a></li><li><a href="https://cloud.tencent.com/edu/learning?from=gw.head" hotrep="hp.footer.navigation.resource.learning">腾讯云学堂</a></li></ul></div>

          </el-col>

          <el-col :span="4">
            <div class="pls-footer-website-group"><h3 class="pls-footer-website-title"><a>支持服务	</a></h3><ul class="pls-footer-website-list"><li><a href="https://cloud.tencent.com/announce" hotrep="hp.footer.navigation.service.announce">公告</a></li><li><a href="https://console.cloud.tencent.com/" hotrep="hp.footer.navigation.service.console">控制台</a></li><li><a href="https://console.cloud.tencent.com/account/renewal" hotrep="hp.footer.navigation.service.renewal">续费</a></li><li><a href="https://cloud.tencent.com/services/security" hotrep="hp.footer.navigation.service.security">安全中心</a></li><li><a href="https://cloud.tencent.com/act/event/report-platform" hotrep="hp.footer.navigation.service.report-platform">举报平台</a></li><li><a href="https://console.cloud.tencent.com/workorder" hotrep="hp.footer.navigation.service.workorder">工单管理</a></li><li><a href="https://cloud.tencent.com/act/event/connect-service" hotrep="hp.footer.navigation.service.connect-service">联系我们</a></li><li><a href="https://cloud.tencent.com/product/ba" hotrep="hp.footer.navigation.service.ba">网站备案</a></li></ul></div>

          </el-col>

          <el-col :span="4">
            <div class="pls-footer-website-group"><h3 class="pls-footer-website-title"><a>腾讯云计算</a></h3><ul class="pls-footer-website-list"><li><a href="https://cloud.tencent.com/product/events" hotrep="hp.footer.navigation.tencentcomputer.events">最新动态</a></li><li><a href="https://cloud.tencent.com/act/free" hotrep="hp.footer.navigation.tencentcomputer.free">免费试用</a></li><li><a href="https://partner.cloud.tencent.com/" hotrep="hp.footer.navigation.tencentcomputer.partner">合作伙伴</a></li><li><a href="https://cloud.tencent.com/customer" hotrep="hp.footer.navigation.tencentcomputer.customer">客户案例</a></li><li><a href="https://cloud.tencent.com/act/event/compliance" hotrep="hp.footer.navigation.tencentcomputer.compliance">合规认证</a></li><li><a href="https://cloud.tencent.com/about" hotrep="hp.footer.navigation.tencentcomputer.about">关于我们</a></li><li><a href="https://techo.cloud.tencent.com/2020/?utm_campaign=ZYQD&amp;utm_source=TXYGW&amp;utm_medium=DHL#/" hotrep="hp.footer.navigation.tencentcomputer.utm_medium">Techo 开发者大会</a></li><li><a href="https://des.cloud.tencent.com/2021/uat/index.html#/" hotrep="hp.footer.navigation.tencentcomputer.uat">腾讯数字生态大会</a></li></ul></div>

          </el-col>

        </el-col>

        <el-col :span="8">
          <div class="pls-footer-website-contact"><h3 class="pls-footer-website-contact-title">关注或联系我们</h3>
            <div class="pls-footer-website-contact-text">添加腾讯云公众号、小程序， 关注云资讯，还可移动管理云服务</div>
            <img src="https://cloudcache.tencentcs.com/qcloud/portal/kit/images/code-1.5ceb23cd.svg" alt="腾讯云公众号">

            <img src="https://cloudcache.tencentcs.com/qcloud/portal/kit/images/code-2.ad70392a.svg" alt="腾讯云小程序">

          </div>
            <div class="pls-footer-website-hotline">
              <h4 class="pls-footer-website-hotline-title">咨询热线：</h4>
              <div class="pls-footer-website-hotline-phone">
              <span>95716</span><i>或</i><span>4009 100 100</span><i>转</i><span>1</span>
              </div>
            </div>

<!--              <a href="https://cloud.tencent.com/act/event/connect-service?from=connect-us" class="pls-footer-website-btn" hotrep="hp.footer.tel">联系我们</a></div><div class="pls-footer-website-rss">-->
<!--              <div class="pls-footer-website-rss-title">腾讯云公众号、小程序——订阅最新动态、远程管理</div><div class="pls-footer-website-rss-search">-->
<!--              <div class="pls-footer-website-rss-mod"><i class="pls-footer-website-rss-meeting"></i></div>-->
<!--              <div class="pls-footer-website-rss-mod"><i class="pls-footer-website-rss-vx"></i></div></div></div>-->
<!--            <div class="pls-footer-website-copyright"><p>Copyright © 2013 - 2022 Tencent Cloud. </p><p>All Rights Reserved. 腾讯云 版权所有</p></div>-->
        </el-col>
      </el-row>
    </el-footer>
    </div>

</template>

<script>
import PanelGroup from './dashboard/PanelGroup'
import LineChart from './dashboard/LineChart'
import RaddarChart from './dashboard/RaddarChart'
import PieChart from './dashboard/PieChart'
import BarChart from './dashboard/BarChart'


const lineChartData = {
  newVisitis: {
    expectedData: [100, 120, 161, 134, 105, 160, 165],
    actualData: [120, 82, 91, 154, 162, 140, 145]
  },
  messages: {
    expectedData: [200, 192, 120, 144, 160, 130, 140],
    actualData: [180, 160, 151, 106, 145, 150, 130]
  },
  purchases: {
    expectedData: [80, 100, 121, 104, 105, 90, 100],
    actualData: [120, 90, 100, 138, 142, 130, 130]
  },
  shoppings: {
    expectedData: [130, 140, 141, 142, 145, 150, 160],
    actualData: [120, 82, 91, 154, 162, 140, 130]
  }
}

export default {
  name: 'Index',
  components: {
    PanelGroup,
    LineChart,
    RaddarChart,
    PieChart,
    BarChart
  },
  data() {
    return {
      //日期
      date_value:null,
      //评分
      rate_value: null,
      //走马灯
      imgList:[
        {
          url:require('@/assets/images/xiangwan.png'),
        },
        {
          url:require('@/assets/images/jiaran.png')
        },
        {
          url:require('@/assets/images/nailin.png')
        },
        {
          url:require('@/assets/images/jiale.png')
        },
        {
          url:require('@/assets/images/beila.png')
        },



      ],
      //技术菜单
      technology_item:['SpringBoot','Spring Security', 'JWT', 'MyBatis', 'Druid']



    }
  },
  methods: {



  }
}
</script>

<style lang="scss" scoped>

.footer_columns{

  background:#eee;
  float:left;
  width:100px;

}
.text-indent{
  background: #DCDFE6;
}
.dashboard-editor-container {
  padding: 32px;
  //
  background-color: rgba(255, 255, 255, .3);
  //background-image: url("../assets/images/data.jpg");
  background-size:100% 100%;
  background-attachment: fixed;
  position: relative;

  .chart-wrapper {
    //
    background: rgba(255, 255, 255, .5);
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
}

.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both
}

.box-card {
  width: 480px;
}
</style>
