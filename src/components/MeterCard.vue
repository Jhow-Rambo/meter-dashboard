<template>
  <div class="flex flex-wrap w-full -mx-2">
    <div class="w-full mt-6 overflow-hidden bg-white rounded shadow-lg">
      <img class="object-cover object-center w-full max-h-56" :src="content.meter_img" alt="Meter" />
      <div class="px-6 py-4">
        <div class="mb-2 text-xl font-bold text-gray-900">
          {{ content.name }}
        </div>
      </div>
      <div class="flex justify-between px-6 pt-4 pb-2">
        <button
          class="px-4 py-2 font-semibold text-green-700 bg-transparent border border-green-500 rounded hover:bg-green-500 hover:text-white hover:border-transparent"
          @click="changeModalValue">
          Abrir
        </button>
        <!-- <button
			class="px-4 py-2 font-semibold text-red-700 bg-transparent border border-red-500 rounded hover:bg-red-500 hover:text-white hover:border-transparent"
			>
			Excluir
			</button> -->
        <button
          class="px-4 py-2 font-semibold text-yellow-700 bg-transparent border border-yellow-500 rounded hover:bg-yellow-500 hover:text-white hover:border-transparent"
          @click="changCoordinatesModalValue">
          Coordenadas
        </button>
      </div>
    </div>

    <MeterModal v-if="modalOpen" @closeModal="changeModalValue" :modalContent="modalContent"
      :meterId="modalContent.id" />
    <CoordinatesModal v-if="coordinatesModal" :modalContent="modalContent" @closeModal="changCoordinatesModalValue" />
  </div>
</template>

<script lang="ts">
import MeterModal from './MeterModal.vue';
import CoordinatesModal from './CoordinatesModal.vue';
import { defineComponent } from "vue";

export default defineComponent({
	data() {
		return {
			modalOpen: false,
			coordinatesModal: false,
			modalContent: { ...this.content }
		}
	},
	components: {
		MeterModal,
		CoordinatesModal
	},
	methods: {
		changeModalValue() {
		this.modalOpen = !this.modalOpen
		},
		changCoordinatesModalValue() {
			this.coordinatesModal = !this.coordinatesModal
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