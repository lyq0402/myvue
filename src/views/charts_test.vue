<template>
  <div>
    <div v-if="pie_display" id="pie-chart" style="width: 600px; height: 400px;"></div>
    <button @click="togglePieDisplay">Toggle Pie Display</button>
  </div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  data() {
    return {
      pie_display: false,
      pie_option: {
        title: {
          text: '订单销售统计',
          subtext: '比例图',
          left: 'center'
        },
        tooltip: {
          trigger: 'item'
        },
        legend: {
          orient: 'vertical',
          left: 'left'
        },
        series: [
          {
            name: 'Access From',
            type: 'pie',
            center: ['50%', '60%'],
            radius: '50%',
            data: [
              { value: 1048, name: 'Search Engine' },
              { value: 735, name: 'Direct' },
              { value: 580, name: 'Email' },
              { value: 484, name: 'Union Ads' },
              { value: 300, name: 'Video Ads' }
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ]
      }
    };
  },
  methods: {
    togglePieDisplay() {
      this.pie_display = !this.pie_display;
    },
    initializePieChart() {
      this.$nextTick(() => {
        let pieDom = document.getElementById('pie-chart');
        if (pieDom) {
          let pieChart = echarts.init(pieDom);
          pieChart.setOption(this.pie_option);
        }
      });
    }
  },
  watch: {
    pie_display(newVal) {
      if (newVal) {
        this.initializePieChart();
      }
    }
  },
  mounted() {
    // 如果初始值为 true，则初始化图表
    if (this.pie_display) {
      this.initializePieChart();
    }
  }
};
</script>

<style scoped>
/* 你的样式 */
</style>
