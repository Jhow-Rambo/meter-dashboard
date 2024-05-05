<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div>
    <Breadcrumb breadcrumb="medidores" />
    <div class="flex justify-between">
      <h4 class="text-gray-700">Medidores Ciclometricos</h4>
      <button type="button"
        class="inline-block px-6 py-2.5 bg-green-500 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-green-600 hover:shadow-lg focus:bg-green-600 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-green-700 active:shadow-lg transition duration-150 ease-in-out"
        @click="changeModalValue">
        Adicionar
      </button>
    </div>
    <div class="w-full flex justify-center">
      <div class="w-full flex gap-20 flex-wrap justify-start">
        <div v-for="(contentItem, index) in contents" :key="index" :content="contentItem" class="w-[400px]">
          <MeterCard :content="contentItem" />
        </div>
        <CreateMeterModalVue v-if="modalOpen" @closeModal="changeModalValue" @updateData="getMeterData" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">

import { defineComponent } from "vue";
import MeterCard from "@/components/MeterCard.vue";
import CreateMeterModalVue from "@/components/CreateMeterModal.vue";
import api from "../api/api";
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

export default defineComponent({
    data() {
        return {
            contents: [
                // Adicione mais objetos aqui para renderizar mais MeterCards
            ],
            modalOpen: false
        }
    },
    components: {
        MeterCard,
        CreateMeterModalVue
    },
    methods: {
        changeModalValue() {
            this.modalOpen = !this.modalOpen
        },
        getMeterData() {
            api.get('/meters')
                .then((response: { data: any; }) => {
                    console.log(response.data);
                    this.contents = response.data;
                    toast("Busca de medidores finalizada.", {
                        theme: "auto",
                        type: "success",
                        autoClose: 5000,
                    });
                })
                .catch((error: any) => {
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
        this.getMeterData();
    }
});

</script>
