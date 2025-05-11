<script setup>
import { ref, provide } from "vue";
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent,
} from "echarts/components";

import VChart, { THEME_KEY } from "vue-echarts";
import { use } from "echarts/core";
import * as echarts from "echarts";
import { CanvasRenderer } from "echarts/renderers";
import { RadarChart } from "echarts/charts";
import { VisualMapComponent } from "echarts/components";

provide(THEME_KEY, "light");
use([
  CanvasRenderer,
  RadarChart,
  TitleComponent,
  VisualMapComponent,
  TooltipComponent,
  LegendComponent,
]);
const option = ref({
  //   color: ['#67F9D8', '#FFE434', '#56A3F1', '#FF917C'],
  legend: {},
  radar: [
    {
      indicator: [
        { text: "Database" },
        { text: "ETL Pipeline" },
        { text: "Software Development" },
        { text: "Model Development" },
        { text: "Storytelling" },
        { text: "Business Insight" },
        { text: "Data Visualization" },
        { text: "Experimentation" },
      ],
      radius: 120,
      startAngle: 90,
      splitNumber: 4,
      axisName: {
        formatter: "{value}",
        color: "#428BD4",
      },
      splitArea: {
        areaStyle: {
          //   color: ['#77EADF', '#26C3BE', '#64AFE9', '#428BD4'],
          shadowColor: "rgba(0, 0, 0, 0.2)",
          shadowBlur: 10,
        },
      },
      axisLine: {
        lineStyle: {
          color: "rgba(211, 253, 250, 0.8)",
        },
      },
      splitLine: {
        lineStyle: {
          color: "rgba(211, 253, 250, 0.8)",
        },
      },
    },
  ],
  series: [
    {
      type: "radar",
      emphasis: {
        lineStyle: {
          width: 4,
        },
      },
      data: [
        {
          value: [100, 100, 70, 40, 30, 30, 30, 30],
          name: "Data Scientist",
        },
        {
          value: [60, 60, 50, 60, 70, 70, 90, 90],
          name: "Data Engineer",
        },
        {
          value: [30, 30, 30, 40, 90, 90, 90, 70],
          name: "Data/Business Analyst",
        },
      ],
    },
  ],
});
</script>

<template>
  <v-chart class="chart" :option="option" autoresize />
</template>
<style scoped>
.chart {
  height: 50vh;
}
</style>
