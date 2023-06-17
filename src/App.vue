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
]);

provide(THEME_KEY, 'roma');

const option = ref({
  title: {
    text: 'Temperature Change in the Coming Week',
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
