<template>
  <div>
    <apexchart width="500" type="area" :options="chartOptions" :series="chartSeries"></apexchart>
  </div>
</template>

<script lang="ts" setup>
import { ref, toRefs, watch } from "vue";
import { defineProps } from 'vue'

interface DataItem {
  x: number;
  y: number;
}

// Definindo as propriedades
const props = defineProps({
  data: Array as () => DataItem[],
  xLabel: String,
  yLabel: String,
});

// Convertendo as propriedades para refs
const { data, xLabel, yLabel } = toRefs(props);

// Definindo as opções do gráfico
const chartOptions = ref({
  chart: {
    id: "vuechart-example",
  },
  xaxis: {
    categories: data?.value?.map((item: DataItem) => item.x) ?? [],
    title: {
      text: xLabel?.value,
    },
  },
  yaxis: {
    title: {
      text: yLabel?.value,
    },
  },
});

// Definindo a série do gráfico
const chartSeries = ref([
  {
    name: "series-1",
    data: data?.value?.map((item: DataItem) => item.y) ?? [],
  },
]);

// Observando mudanças na propriedade 'data'
watch(data, (newData) => {
  chartOptions.value.xaxis.categories = newData.map((item: DataItem) => item.x);
  chartSeries.value[0].data = newData.map((item: DataItem) => item.y);
});
</script>

<style scoped>
::v-deep .apexcharts-xaxis-title {
  transform: translateY(85px);
}
</style>