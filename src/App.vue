<template>
  <div class="cell" v-for="(value, key) in model_options" :key="key">
    <v-chart
      class="chart"
      v-for="(value, key) in value"
      :key="key"
      :option="value"
      autoresize
    ></v-chart>
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
  legend: {},
  toolbox: {
    show: true,
    feature: {
      dataZoom: {},
      dataView: { readOnly: false },
      saveAsImage: {},
    },
  },
  xAxis: {
    type: 'value',
  },
  yAxis: {
    type: 'value',
  },
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
        model_options[model] = {};
        for (const key in res.data) {
          model_options[model][key] = JSON.parse(
            JSON.stringify(option_template)
          );
          model_options[model][key].series = res.data[key];
          model_options[model][key].title = {
            text: key,
          };
          console.log('构造报表配置', model_options[model]);
        }
      });
    });
  });
});
</script>

<style scoped>
.chart {
  height: 100vh;
  width: 50vw;
}
.cell {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
}
</style>
