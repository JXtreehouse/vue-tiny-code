<!--
 * @Author: jsjzh
 * @Email: kimimi_king@163.com
 * @Date: 2019-02-18 10:43:52
 * @LastEditors: jsjzh
 * @LastEditTime: 2019-03-11 15:04:34
 * @Description: custom-report-bar-pie-module
 -->
<template>
  <div style="display: flex">
    <default-bar-chart ref="bar-chart" style="flex: 6" @reload="reloadChart"/>
    <default-pie-chart ref="pie-chart" style="flex: 4" @reload="reloadChart"/>
  </div>
</template>

<script>
import defaultBarChart from "./default-bar-chart";
import defaultPieChart from "./default-pie-chart";

import { barFirstOption, pieSecondOption } from "./js/variable";

export default {
  name: "custom-report-bar-pie-module",
  props: {
    reportData: {
      type: Array,
      default() {
        return [];
      }
    }
  },
  watch: {
    reportData: {
      handler(newVal, oldVal) {
        // 父组件数据更新触发 echart 更新
        this.renderChart(newVal);
      }
    }
  },
  components: { defaultBarChart, defaultPieChart },
  methods: {
    // 开发模式下，热加载组件触发 echart 更新
    reloadChart() {
      Object.keys(this.$refs).forEach(chart => {
        switch (chart) {
          case "bar-chart":
            this.$refs[chart].setOption(barFirstOption);
            break;
          case "pie-chart":
            this.$refs[chart].setOption(pieSecondOption);
            break;
        }
      });
      this.reportData.length && this.renderChart(this.reportData);
    },
    renderChart(option) {
      Object.keys(this.$refs).forEach(chart => {
        switch (chart) {
          case "bar-chart":
            this.$refs[chart].setOption({
              xAxis: {
                data: option.map(item => item.name)
              },
              series: {
                data: option.map(item => item.value)
              }
            });
            break;
          case "pie-chart":
            this.$refs[chart].setOption({
              series: {
                data: option.map(item => ({
                  name: item.name,
                  value: item.value
                }))
              }
            });
            break;
        }
      });
    }
  },
  mounted() {
    this.reloadChart();
  }
};
</script>

<style lang="scss" scoped>
@import "./css/index.scss";
</style>

