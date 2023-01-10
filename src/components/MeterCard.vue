<template>
  <div class="flex flex-wrap -mx-2 grid-rows-3">
    <div class="max-w-sm mt-6 overflow-hidden bg-white rounded shadow-lg sm:w-1/2 xl:w/2 sm:mx-1 xl:mx-1 mx-auto">
      <img class="w-full" src="../assets/medidor1.jpg" alt="Meter" />
      <div class="px-6 py-4">
        <div class="mb-2 text-xl font-bold text-gray-900">
          {{ content.title }}
        </div>
      </div>
      <div class="px-6 pt-4 pb-2 flex justify-between">
        <button
          class="bg-transparent hover:bg-green-500 text-green-700 font-semibold hover:text-white py-2 px-4 border border-green-500 hover:border-transparent rounded"
          @click="changeModalValue"
          >
          Abrir
        </button>
        <button
          class="bg-transparent hover:bg-red-500 text-red-700 font-semibold hover:text-white py-2 px-4 border border-red-500 hover:border-transparent rounded"
          >
          Excluir
        </button>
      </div>
    </div>

    <MeterModal v-if="modalOpen" @closeModal="changeModalValue" :modalContent="modalContent"/>
  </div>
</template>

<script lang="ts">
import MeterModal from './MeterModal.vue';
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    return {
      modalOpen: false,
      modalContent: {
        title: this.content.title
      }
    }
  },
  components: {
    MeterModal
  },
  methods: {
    changeModalValue() {
      this.modalOpen = !this.modalOpen
    }
  },
  props: ['content']
});
</script>


<style>
.modal {
  transition: opacity 0.25s ease;
}
</style>