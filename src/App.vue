<template>
  <div class="cell" v-for="(value, key) in model_options" :key="key">
    <v-chart class="chart" :option="value" autoresize></v-chart>
  </div>
  <div class="cell" v-for="(value, key) in model_options" :key="key">
    <v-chart class="chart" :option="value" autoresize></v-chart>
  </div>
</template>

<script setup>
import axios from 'axios';
import { use } from 'echarts/core';
import { CanvasRenderer } from 'echarts/renderers';
import { LineChart } from 'echarts/charts';
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent,
  GridComponent,
  ToolboxComponent,
} from 'echarts/components';
import VChart, { THEME_KEY } from 'vue-echarts';
import { ref, provide, reactive, onMounted } from 'vue';

use([
  CanvasRenderer,
  LineChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent,
  GridComponent,
  ToolboxComponent,
]);

provide(THEME_KEY, 'roma');

let model_options = reactive({});

const option_template = {
  title: {
    text: 'Data',
  },
  tooltip: {
    trigger: 'axis',
    textStyle: {
      fontSize: 10,
    },
    itemHeight: 2,
  },
  legend: {
    textStyle: {
      //图例字体大小
      fontSize: 10,
    },
    //图例大小
    itemHeight: 2,
    //图例位置
    bottom: '-15%',
  },
  toolbox: {
    show: true,
    feature: {
      dataZoom: {},
      dataView: { readOnly: false },
      saveAsImage: {},
    },
  },
  graphic: {
    type: 'text',
    left: 'center',
    top: '5%',
    style: {
      text: 'tokens         epoch',
      textAlign: 'center',
      fill: '#333',
      fontSize: 10,
      fontWeight: 700,
    },
  },
  xAxis: [
    {},
    {
      gridIndex: 1,
    },
  ],
  yAxis: [
    {},
    {
      gridIndex: 1,
    },
  ],
  grid: [
    {
      right: '55%',
      bottom: '25%',
    },
    {
      left: '55%',
      bottom: '25%',
    },
  ],
  series: [
    {
      name: 'Highest',
      type: 'line',
      data: [
        [0, 0],
        [1, 1],
        [2, 3.5],
        [2, 5],
      ],
    },
  ],
};

onMounted(() => {
  axios.get('/models').then((res) => {
    console.log('请求模型种类', res);
    res.data.forEach((model) => {
      axios.get('/data/' + model).then((res) => {
        console.log('请求模型数据', res);
        model_options[model] = JSON.parse(JSON.stringify(option_template));
        model_options[model].series = res.data;
        model_options[model].title = {
          text: model,
        };
        console.log('模型配置', model_options[model]);
      });
    });
  });
});
</script>

<style scoped>
body {
  padding-top: 40px;
  width: 100vw;
}
.chart {
  height: 100vh;
  width: 100vw;
  overflow: visible;
}
.cell {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  overflow: visible;
}
</style>
