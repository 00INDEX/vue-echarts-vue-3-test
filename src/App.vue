<template>
  <v-chart class="chart" :option="option" autoresize />
</template>

<script setup>
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
import { ref, provide } from 'vue';

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

const refresh = (option) => {
  option.value.series.push({
    name: 'Lowest',
    type: 'line',
    data: [
      [0, -1],
      [1, -1],
      [2, 3.5]
    ],
  });
};

const option = ref({
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
      restore: {},
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
        [2,5]
      ],
    },
    {
      name: 'Lowest',
      type: 'line',
      data: [
        [0,2],
        [1, 2],
        [2, 3.5],
        [6,5.5]
      ],
    },
  ],
});
</script>

<style scoped>
.chart {
  height: 100vh;
}
</style>
