<template>
  <div class="word-cloud-container">
    <div ref="chartRef" :style="{ width: width, height: height }"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, watch } from "vue";
import * as echarts from "echarts";
import "echarts-wordcloud";

const data = ref([
  { id: 1, name: "Python", value: 75 },
  { id: 2, name: "SQL", value: 70 },
  { id: 3, name: "Apache Spark", value: 45 },
  { id: 4, name: "Hadoop", value: 43 },
  { id: 5, name: "Airflow", value: 40 },
  { id: 6, name: "Kafka", value: 38 },
  { id: 7, name: "Docker", value: 37 },
  { id: 8, name: "Kubernetes", value: 35 },
  { id: 9, name: "ETL Tools (e.g., Informatica, Talend)", value: 33 },
  { id: 10, name: "AWS", value: 32 },
  { id: 11, name: "GCP", value: 30 },
  { id: 12, name: "Azure", value: 28 },
  { id: 13, name: "PostgreSQL", value: 27 },
  { id: 14, name: "MongoDB", value: 25 },
  { id: 15, name: "Redis", value: 23 },
  { id: 16, name: "Snowflake", value: 22 },
  { id: 17, name: "Redshift", value: 20 },
  { id: 18, name: "BigQuery", value: 18 },
  { id: 19, name: "Data Modeling", value: 17 },
  { id: 20, name: "Version Control (e.g., Git)", value: 15 },
  { id: 21, name: "Presto", value: 14 },
  { id: 22, name: "Looker", value: 13 },
  { id: 23, name: "Superset", value: 12 },
  { id: 24, name: "Streamlit", value: 11 },
  { id: 25, name: "ElasticSearch", value: 10 },
  { id: 26, name: "Dask", value: 9 },
  { id: 27, name: "Ray", value: 8 },
  { id: 28, name: "MLflow", value: 7 },
  { id: 29, name: "SageMaker", value: 6 },
  { id: 30, name: "FastAPI", value: 5 },
]);

const props = defineProps({
  width: {
    type: String,
    default: "400px",
  },
  height: {
    type: String,
    default: "300px",
  },
  sizeRange: {
    type: Array,
    default: () => [6, 48],
  },
  rotationRange: {
    type: Array,
    default: () => [-90, 90],
  },
  colors: {
    type: Array,
    default: () => [
      "#004D66",
      "#005B5E",
      "#003D3A",
      "#002627",
      "#00394D",
      "#003A40",
      "#004C39",
      "#00544A",
      "#003F47",
      "#004D4D",
      "#00008B",
      "#0000CD",
      "#4169E1",
    ],
  },
  shape: {
    type: String,
    default: "circle",
  },
});

// 定义事件
const emit = defineEmits(["click"]);

const chartRef = ref(null);
let chart = null;

// 初始化词云图
const initChart = () => {
  if (!chartRef.value) return;

  chart = echarts.init(chartRef.value);
  const option = {
    backgroundColor: "transparent",
    tooltip: {
      show: true,
      formatter: "{b}: {c}",
      textStyle: {
        color: "#5BE9FF",
      },
      backgroundColor: "rgba(0,0,0,0.7)",
      borderColor: "#5BE9FF",
      borderWidth: 1,
    },
    series: [
      {
        type: "wordCloud",
        shape: props.shape,
        left: "center",
        top: "center",
        width: "90%",
        height: "90%",
        sizeRange: props.sizeRange,
        rotationRange: props.rotationRange,
        rotationStep: 45,
        gridSize: 8,
        drawOutOfBound: false,
        textStyle: {
          fontFamily: "sans-serif",
          fontWeight: "bold",
          color: function () {
            return props.colors[
              Math.floor(Math.random() * props.colors.length)
            ];
          },
        },
        emphasis: {
          textStyle: {
            shadowBlur: 10,
            shadowColor: "rgba(0, 255, 255, 0.5)",
          },
        },
        data: data.value.sort((a, b) => b.value - a.value),
      },
    ],
  };

  chart.setOption(option);

  // 注册点击事件
  chart.on("click", (params) => {
    emit("click", {
      name: params.name,
      value: params.value,
      dataIndex: params.dataIndex,
      data: data,
    });
  });
};

const handleResize = () => {
  chart && chart.resize();
};

watch(
  data,
  () => {
    initChart();
  },
  { deep: true },
);

onMounted(() => {
  initChart();
  window.addEventListener("resize", handleResize);
});

onBeforeUnmount(() => {
  if (chart) {
    chart.dispose();
    chart = null;
  }
  window.removeEventListener("resize", handleResize);
});
</script>
<style scoped>
.word-cloud-container {
  width: 400px;
  height: 300px;
  margin: 20px 0;
  border-radius: 4px;
}
</style>
