<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div>
    <Breadcrumb breadcrumb="medidores" />
    <div class="flex justify-between">
      <h4 class="text-gray-700">Medidor Ciclometrico</h4>
          <button type="button"
            class="inline-block px-6 py-2.5 bg-green-500 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-green-600 hover:shadow-lg focus:bg-green-600 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-green-700 active:shadow-lg transition duration-150 ease-in-out"
            @click="changeModalValue"
            >
            Adicionar
          </button>
    </div>
    <MeterCard :content=content />
    <CreateMeterModalVue v-if="modalOpen" :modal-content="content" @closeModal="changeModalValue"/>
  </div>
</template>

<script lang="ts">

import { defineComponent } from "vue";
import MeterCard from "@/components/MeterCard.vue";
import CreateMeterModalVue from "@/components/CreateMeterModal.vue";

export default defineComponent({
  data() {
    return {
      content: {
        title: 'Medidor Vector 3C',
        img: '../assets/medidor1.jpg'
      },
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
  }
});

</script>
