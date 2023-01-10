 <template>
  <div>
    <apexchart
      width="350"
      type="line"
      :options="options"
      :series="series"
    ></apexchart>
  </div>
</template>

<script lang="ts" setup>
import { ref } from "vue";
const options = ref({
  chart: {
    id: "vuechart-example",
  },
  xaxis: {
    categories: [1, 2, 3, 4, 5, 6, 7],
  },
});
const series = ref([
  {
    type: "line",
    name: "chart-1",
    data: [0, 1, 1, 1, 0, 1, 1],
  },
]);
</script>