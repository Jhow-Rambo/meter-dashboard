<template>
  <div>
    <apexchart width="350" type="line" :options="options" :series="series"></apexchart>
  </div>
</template>

<script>
export default {
  props: {
    chartData: {
      type: Array,
      default: () => [0, 0, 0, 0, 0, 0, 0],
    },
  },
  data() {
    return {
      options: {
        chart: {
          id: "vuechart-example",
        },
        xaxis: {
          categories: [1, 2, 3, 4, 5, 6, 7],
        },
      },
      series: [
        {
          type: "line",
          name: "chart-1",
          data: [],
        },
      ],
    };
  },
  watch: {
    chartData: {
      immediate: true,
      handler(newChartData) {
        if (Array.isArray(newChartData)) {
          this.series[0].data = newChartData;
        } else {
          this.series[0].data = [0, 0, 0, 0, 0, 0, 0];
        }
      },
    },
  },
};
</script>