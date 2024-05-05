<template>
  <div>
    <apexchart width="456" type="donut" :options="chartOptions" :series="series"></apexchart>
  </div>
</template>

<script lang="ts" setup>
import { ref, toRefs, watch } from "vue";
import { defineProps } from 'vue'

interface SeriesItem {
  value: number;
  label: string;
}

// Definindo as propriedades
const props = defineProps({
  seriesData: Array as () => SeriesItem[],
});

// Convertendo as propriedades para refs
const { seriesData } = toRefs(props);

// Definindo as opções do gráfico
const chartOptions = ref({ labels: [] });

// Definindo a série do gráfico
const series = ref([]);

// Observando mudanças na propriedade 'seriesData'
watch(() => seriesData.value, (newSeriesData) => {
  console.log('newSeriesData:', newSeriesData);
  if (newSeriesData) {
    chartOptions.value = { labels: newSeriesData.map((item: SeriesItem) => item.label) };
    series.value = newSeriesData.map((item: SeriesItem) => item.value);
  }
  console.log('chartOptions:', chartOptions.value);
  console.log('series:', series.value);
});
</script>