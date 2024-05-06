<template>
  <div>
    <Breadcrumb breadcrumb="" />
    <div class="mt-4">
      <div class="flex flex-wrap -mx-6">
        <div class="w-full px-6 sm:w-1/2 xl:w-1/2">
          <div class="flex items-center px-5 py-6 bg-white rounded-md shadow-sm">
            <div class="p-3 bg-indigo-600 bg-opacity-75 rounded-full">
              <img src="../assets/electric-meter.png" class="w-8" />
            </div>

            <div class="mx-5">
              <h4 class="text-2xl font-semibold text-gray-700">{{ activateMeters }}</h4>
              <div class="text-gray-500">Medidores Ativos</div>
            </div>
          </div>
        </div>

        <div class="w-full px-6 mt-6 sm:w-1/2 xl:w-1/2 sm:mt-0">
          <div class="flex items-center px-5 py-6 bg-white rounded-md shadow-sm">
            <div class="p-3 bg-blue-600 bg-opacity-75 rounded-full">
              <img src="../assets/data-storage.png" class="w-8" />
            </div>

            <div class="mx-5">
              <h4 class="text-2xl font-semibold text-gray-700">{{ weeklyReadings }}</h4>
              <div class="text-gray-500">Leituras semanal</div>
            </div>
          </div>
        </div>

        <div class="w-full px-6 mt-6 sm:w-1/2 xl:w-1/2 sm:mt-0 max-h-96">
          <div class="w-full h-full gap-3 px-6 py-6 my-6 overflow-hidden bg-white rounded-md shadow">
            <div class="flex flex-col justify-center gap-4">
              <div class="text-gray-700">Atividade Semanal</div>
              <AreaChart class="m-auto" :data="areaChartData" xLabel="Dia" yLabel="Medidores" />
            </div>
          </div>
        </div>

        <div class="w-full px-6 mt-6 sm:w-1/2 xl:w-1/2 sm:mt-0 max-h-96">
          <div class="w-full h-full gap-3 px-6 py-6 my-6 overflow-hidden bg-white rounded-md shadow">
            <div class="flex flex-col justify-center gap-4">
              <div class="text-gray-700">Medidores por Setor</div>

              <DonutChart class="m-auto" :seriesData="donutChartData" />
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="mt-8"></div>
  </div>
</template>

<script>
import { ref } from "vue";
import Breadcrumb from "../partials/Breadcrumb.vue";

import DonutChart from "@/components/charts/DonutChart.vue";
import AreaChart from "../components/charts/AreaChart.vue";

import api from "../api/api";
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

const testUser = {
  name: "John Doe",
  email: "john@example.com",
  title: "Software Engineer",
  title2: "Web dev",
  status: "Active",
  role: "Owner",
};

export default {
  name: 'DashboardView',
  components: {
    Breadcrumb,
    DonutChart,
    AreaChart
  },
  data() {
    return {
      areaChartData: [
        { x: 1, y: 0 },
        { x: 2, y: 0 },
        { x: 3, y: 0 },
        { x: 4, y: 0 },
        { x: 5, y: 0 },
        { x: 6, y: 0 },
        { x: 7, y: 0 },
      ],
      donutChartData: [
        { value: 30, label: '' },
        { value: 30, label: '' },
        { value: 40, label: '' },
      ],
      activateMeters: 0,
      weeklyReadings: 0,
    };
  },
  methods: {
    getOverviewData() {
      api.get('/overview')
        .then((response) => {
          console.log(response.data);

          this.areaChartData = response.data.daily_inference_count;
          this.donutChartData = response.data.meter_percent_by_location;
          this.activateMeters = response.data.active_meters;
          this.weeklyReadings = response.data.weekly_inference_count;

          toast("Dados de relatório carregado com sucesso!", {
            theme: "auto",
            type: "success",
            autoClose: 5000,
          });
        })
        .catch((error) => {
          console.error(error);
          toast("Ocorreu um erro", {
            theme: "auto",
            type: "error",
            autoClose: 5000,
          });
        });
    }
  },
  mounted() {
    this.getOverviewData();
  },
  setup() {
    const users = ref([...Array(10).keys()].map(() => testUser));

    return {
      users,
    };
  }
};
</script>