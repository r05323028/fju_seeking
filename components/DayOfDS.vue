<script setup>
import { ref, provide } from "vue";
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent,
} from "echarts/components";

import VChart, { THEME_KEY } from "vue-echarts";
import { use } from "echarts/core";
import { CanvasRenderer } from "echarts/renderers";
import { SunburstChart } from "echarts/charts";
import { VisualMapComponent } from "echarts/components";

provide(THEME_KEY, "light");
use([
  CanvasRenderer,
  SunburstChart,
  TitleComponent,
  VisualMapComponent,
  TooltipComponent,
  LegendComponent,
]);
const data = [
  {
    name: "Code",
    value: 50,
    itemStyle: {
      color: "red",
    },
    children: [
      {
        name: "Data Engineering",
        value: 70,
      },
      {
        name: "Model/Research Implementation",
        value: 15,
      },
      {
        name: "PoC/Data Visualization",
        value: 15,
      },
    ],
  },
  {
    name: "Research",
    value: 25,
  },
  {
    name: "Reporting/Meeting",
    value: 25,
  },
];
const option = ref({
  visualMap: {
    type: "continuous",
    min: 0,
    max: 100,
    inRange: {
      color: ["#2F93C8", "#AEC48F", "#FFDB5C", "#F98862"],
    },
  },
  series: {
    type: "sunburst",
    data: data,
    radius: [0, "90%"],
    label: {
      rotate: "radial",
    },
  },
});
</script>

<template>
  <div>
    <v-chart class="chart" :option="option" autoresize />
  </div>
</template>
<style scoped>
.chart {
  height: 50vh;
}
</style>
