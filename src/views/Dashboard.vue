<template>
  <div>
    <Breadcrumb breadcrumb="" />
    <div class="mt-4">
      <div class="flex flex-wrap -mx-6">
        <div class="w-full px-6 sm:w-1/2 xl:w-1/2">
          <div
            class="flex items-center px-5 py-6 bg-white rounded-md shadow-sm"
          >
            <div class="p-3 bg-indigo-600 bg-opacity-75 rounded-full">
              <img src="../assets/electric-meter.png" width="32px"/>
            </div>

            <div class="mx-5">
              <h4 class="text-2xl font-semibold text-gray-700">5</h4>
              <div class="text-gray-500">Medidores Ativos</div>
            </div>
          </div>
        </div>

        <div class="w-full px-6 mt-6 sm:w-1/2 xl:w-1/2 sm:mt-0">
          <div
            class="flex items-center px-5 py-6 bg-white rounded-md shadow-sm"
          >
            <div class="p-3 bg-blue-600 bg-opacity-75 rounded-full">
              <img src="../assets/data-storage.png" width="32px" />
            </div>

            <div class="mx-5">
              <h4 class="text-2xl font-semibold text-gray-700">457</h4>
              <div class="text-gray-500">Leituras semanal</div>
            </div>
          </div>
        </div>

        <div class="w-full px-6 mt-6 sm:w-1/2 xl:w-1/2 sm:mt-0">
          <div class="
              my-6
              overflow-hidden
              bg-white
              rounded-md
              shadow
              gap-3
              w-full
              px-6
            ">
            <div class="flex justify-center">
              <AreaChart />
            </div>
          </div>
        </div>

        <div class="w-full px-6 mt-6 sm:w-1/2 xl:w-1/2 sm:mt-0">
          <div class="
            my-6
            overflow-hidden
            bg-white
            rounded-md
            shadow
            gap-3
            w-full
            px-6
          ">
            <div class="flex justify-center">
              <DonutChart />
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="mt-8"></div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Breadcrumb from "../partials/Breadcrumb.vue";

import DonutChart from "@/components/charts/DonutChart.vue";
import AreaChart from "../components/charts/AreaChart.vue";

interface User {
  name: string;
  email: string;
  title: string;
  title2: string;
  status: string;
  role: string;
}

const testUser: User = {
  name: "John Doe",
  email: "john@example.com",
  title: "Software Engineer",
  title2: "Web dev",
  status: "Active",
  role: "Owner",
};

const users = ref<User[]>([...Array(10).keys()].map(() => testUser));
</script>
