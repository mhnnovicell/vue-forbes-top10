<template>
  <BarChart
    v-if="loaded"
    :chartData="testData"
    :options="options"
    v-motion-fade
  />
</template>

<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import { ref, onMounted, computed, reactive } from 'vue';
import { BarChart } from 'vue-chart-3';
import { Chart, ChartType, registerables } from 'chart.js';

Chart.register(...registerables);

const loaded = ref(false);
const chartData = ref([]);
const chartDataWorth = ref([] as any);
const chartLabels = ref([] as any);
const chartImages = ref([] as any);

const options = ref({
  responsive: true,
  plugins: {
    legend: {
      position: 'top',
    },
    title: {
      display: true,
      text: 'Forbes top 10 list of richest people in the world',
    },
  },
});

onMounted(async () => {
  loaded.value = false;

  const response = await fetch(
    'https://forbes400.herokuapp.com/api/forbes400?limit=10'
  );
  const data = await response.json();
  chartData.value = data;
  loaded.value = true;

  const test = [] as any;

  data.forEach((element: any) => {
    let calculatedWorth = element.finalWorth / 1000;
    chartLabels.value.push(element.person.name);

    chartDataWorth.value.push(calculatedWorth);

    chartImages.value.push(element.squareImage);

    console.log(test, 'chartDataWorth.value');

    console.log(element.finalWorth);
  });
});

const testData = computed(() => ({
  labels: chartLabels.value,
  datasets: [
    {
      label: 'Number of millions',
      data: chartDataWorth.value,
      backgroundColor: ['#77CEFF', '#0079AF', '#123E6B', '#97B0C4', '#A5C8ED'],
    },
  ],
}));
</script>
