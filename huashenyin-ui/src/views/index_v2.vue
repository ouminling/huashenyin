<template style="background:#f5f6fa;">
  <div class="dashboard-editor-container">

    <!--走马灯-->
    <el-carousel type="card" height="250px">
      <el-carousel-item v-for="(img,index) in imgList" :key="index">
        <img :src="img.url" width="100%" height="100%" >
      </el-carousel-item>
    </el-carousel>

    <el-row :gutter="[16, 16]" style="margin:auto ;">
<!--      &lt;!&ndash; 顶部列表 &ndash;&gt;
      <el-button-col :span="24" style="padding-bottom:0;">
        <el-row :gutter="[16, 16]" class="top-list">
          <el-col :xl="6" :lg="8" :md="16" style="height:164px;">
            <a style="display: block;height: 148px;overflow: hidden;border-radius: 4px;background: #516bf7;"
              ><img src="@/assets/images/banner1.png" /></a>
          </el-col>-->

    </el-row>

    <panel-group @handleSetLineChartData="handleSetLineChartData" />

    <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
      <line-chart :chart-data="lineChartData" />
    </el-row>

    <el-row :gutter="32">
      <el-col :xs="24" :sm="24" :lg="8">
        <div class="chart-wrapper">
          <raddar-chart />
        </div>
      </el-col>
      <el-col :xs="24" :sm="24" :lg="8">
        <div class="chart-wrapper">
          <pie-chart />
        </div>
      </el-col>
      <el-col :xs="24" :sm="24" :lg="8">
        <div class="chart-wrapper">
          <bar-chart />
        </div>
      </el-col>
    </el-row>


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
      lineChartData: lineChartData.newVisitis,
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



      ]
    }
  },
  methods: {
    handleSetLineChartData(type) {
      this.lineChartData = lineChartData[type]
    }
  }
}
</script>

<style lang="scss" scoped>


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

@media (max-width:1024px) {
  .chart-wrapper {
    padding: 8px;
  }
}

.el-carousel__item  {
  color: #475669;
  font-size: 14px;
  opacity: 0.90;
  line-height: 150px;
  margin: 0;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n+1) {
  background-color: #d3dce6;
}
</style>
