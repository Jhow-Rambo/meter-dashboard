<template>
  <div :class="`modal z-50 fixed w-full h-full top-0 left-0 flex items-center justify-center`">
    <div @click="changeModalValue" class="absolute w-full h-full bg-gray-900 opacity-50 modal-overlay"></div>

    <div class="z-50 w-11/12 mx-auto overflow-y-auto bg-white rounded shadow-lg modal-container md:max-w-md">
      <div class="px-6 py-4 text-left modal-content">
        <div class="flex items-center justify-between pb-3">
          <p class="text-2xl font-bold">{{ modalContent.name }}</p>
          <div class="z-50 cursor-pointer modal-close" @click="changeModalValue">
            <svg class="text-black fill-current" xmlns="http://www.w3.org/2000/svg" width="18" height="18"
              viewBox="0 0 18 18">
              <path
                d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z" />
            </svg>
          </div>
        </div>

        <div class="flex">
          <h1 class="font-bold text-1xl">Status:</h1>
          <h1 class="pl-1" :class="statusColor">{{ modalContent.status }}</h1>
          <!-- <h1 class="pl-1 text-yellow-600">Aguardando Coordenadas</h1> -->
        </div>

        <div class="flex">
          <h1 class="font-bold text-1xl">Localização:</h1>
          <h1 class="pl-1">{{ modalContent.location }}</h1>
        </div>

        <div class="flex">
          <h1 class="font-bold text-1xl">Ultima Leitura:</h1>
          <h1 class="pl-1">{{ modalContent.inference[modalContent.inference.length - 1]?.value ?
            modalContent.inference[modalContent.inference.length - 1].value : '-' }}</h1>
        </div>

        <img :src="modalContent.inference[modalContent.inference.length - 1]?.image" class="w-full mx-auto" />

        <div class="flex">
          <h1 class="font-bold text-1xl">Atividade semanal:</h1>
        </div>

        <div class="mx-3">
          <LineChart :chartData="parsedWeeklyActivity" />
        </div>

        <div class="flex justify-end pt-2">
          <button @click="changeModalValue"
            class="px-6 py-3 font-medium tracking-wide text-white bg-indigo-600 rounded-md hover:bg-indigo-500 focus:outline-none">
            Fechar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import LineChart from "./charts/LineChart.vue";
import api from "../api/api";
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

export default defineComponent({
	data() {
    return {
      modalContent: {
        id: 0,
        name: '',
        status: '',
        location: '',
        inference: [],
        weekly_activity: [],
      }
    };
	},
    emits: ["closeModal"],
    computed: {
        statusColor() {
            switch (this.modalContent.status) {
                case 'Aguardando Coordenadas':
                    return 'text-yellow-600';
                case 'Inativo':
                    return 'text-red-600';
                case 'Ativo':
                    return 'text-green-600';
                default:
                    return '';
            }
      },
      parsedWeeklyActivity() {
        if (typeof this.modalContent.weekly_activity === 'string') {
          if (!this.modalContent.weekly_activity) {
            return [0, 0, 0, 0, 0, 0, 0];
          }
          try {
            return JSON.parse(this.modalContent.weekly_activity);
          } catch (error) {
            console.error('Error parsing weekly_activity:', error);
          }
        }
        return [];
      },
        // outras propriedades computadas...
    },
	methods: {
		changeModalValue() {
			this.$emit("closeModal");
		},
        getMeterData() {
          api.get(`/meters/${this.meterId}`)
                .then((response: { data: any; }) => {
                    this.modalContent = response.data;

                    toast("Busca de dados finalizada!", {
                        theme: "auto",
                        type: "success",
                         
                        autoClose: 2000,
                    });
                })
                .catch((error: any) => {
                    console.error(error);
                    toast("Ocorreu um erro", {
                        theme: "auto",
                        type: "error",
                         
                        autoClose: 2000,
                    });
                });
		}
	},
	mounted() {
		this.getMeterData();
  },
  props: ['meterId'],
	components: { LineChart }
});
</script>
