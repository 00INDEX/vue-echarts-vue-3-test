<template>
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
  },
  legend: {
    textStyle: {
      //图例字体大小
      fontSize: 10,
    },
    //图例大小
    itemHeight: 2,
    //图例位置
    top: '100%',
  },
  toolbox: {
    show: true,
    feature: {
      dataZoom: {},
      dataView: { readOnly: false },
      saveAsImage: {},
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
    },
    {
      left: '55%',
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
.chart {
  height: 90vh;
  width: 140vw;
}
.cell {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
}
</style>
