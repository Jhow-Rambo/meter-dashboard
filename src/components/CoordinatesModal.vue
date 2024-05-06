<template>
  <div :class="`modal z-50 fixed w-full h-full top-0 left-0 flex items-center justify-center`">
    <div @click="changeModalValue" class="absolute w-full h-full bg-gray-900 opacity-50 modal-overlay"></div>

    <div class="z-50 w-fit mx-auto overflow-y-auto bg-white rounded shadow-lg modal-container">
      <div class="flex flex-col gap-4 px-6 py-4 text-left modal-content">
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

        <div v-show="imageReference">
          <div ref="drawArea" style="position: relative;" @mousedown="startDrawing" @mousemove="draw"
            @mouseup="endDrawing">
            <img :src="imageReference" alt="Your Image" style="position: relative; z-index: 1;" />

            <svg style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 3;">
              <rect v-if="rectangle" :x="rectangle.x" :y="rectangle.y" :width="rectangle.width"
                :height="rectangle.height" style="fill: lime; stroke: purple; stroke-width: 1; fill-opacity: 0.5;" />
            </svg>
          </div>
        </div>

        <div v-show="!imageReference">
          Sem imagem de referência
        </div>

        <div class="flex justify-between">
          <div class="flex justify-end pt-2">
            <button @click="saveCoordinates"
              class="px-6 py-3 font-medium tracking-wide text-white bg-green-600 rounded-md hover:bg-green-500 focus:outline-none">
              Salvar
            </button>
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
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import api from "../api/api";
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

export default defineComponent({
  data() {
    return {
      drawing: false,
      startX: 0,
      startY: 0,
      currentX: 0,
      currentY: 0,
      rectangle: null as { x: number; y: number; width: number; height: number } | null,
      drawArea: null as HTMLElement | null,
      imageReference: null as string | null
    };
  },
  emits: ["closeModal"],
  mounted() {
    this.drawArea = this.$refs.drawArea as HTMLElement;

    this.getMeterImageReference();
    this.getCoordinates();
  },
  methods: {
    getMeterImageReference() {
      api.get(`/reference-image/${this.modalContent.id}`)
        .then((response: { data: any; }) => {
          console.log(response.data);

          this.imageReference = response.data.reference_image;

          toast("Imagem de referência recuperada com sucesso!", {
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
    },
    getCoordinates() {
      api.get(`/coordinates/${this.modalContent.id}`)
        .then((response: { data: any; }) => {
          console.log(response.data);

          // this.rectangle = response.data.coordinates;
          const [x, y, width, height] = response.data.coordinates;
          this.rectangle = { x, y, width, height };
        })
        .catch((error: any) => {
          console.error(error);
          toast("Ocorreu um erro", {
            theme: "auto",
            type: "error",
            autoClose: 2000,
          });
        });
    },
    saveCoordinates() {
      if (!this.imageReference) {
        toast("Sem imagem de referência", {
          theme: "auto",
          type: "error",
          autoClose: 2000,
        });
        return;
      } else {
        if (!this.rectangle) {
          toast("Sem coordenadas", {
            theme: "auto",
            type: "error",
            autoClose: 2000,
          });
          return;
        } else {
          const { x, y, width, height } = this.rectangle;
          api.post(`/coordinates`, {
            id: this.modalContent.id,
            coordinates: [x, y, width, height]
          })
            .then((response: { data: any; }) => {
              console.log(response.data);

              toast("Coordenadas salvas com sucesso!", {
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
      }
    },
    changeModalValue() {
      this.$emit("closeModal");
    },
    startDrawing(event: { stopPropagation: () => void; clientX: number; clientY: number; }) {
      event.stopPropagation();
      console.log('startDrawing');
      this.drawing = true;
      const rect = this.drawArea?.getBoundingClientRect();
      if (rect) { 
        this.startX = event.clientX - rect.left;
        this.startY = event.clientY - rect.top;
      }
      this.rectangle = null;
    },
    draw(event: { clientX: number; clientY: number; }) {
      if (!this.drawing) return;
      console.log('draw');
      const rect = this.drawArea?.getBoundingClientRect();
      if (rect) {
        this.currentX = event.clientX - rect.left;
        this.currentY = event.clientY - rect.top;
      }

      // Calculate x, y, width and height
      let x = Math.min(this.startX, this.currentX);
      let y = Math.min(this.startY, this.currentY);
      let width = Math.abs(this.currentX - this.startX);
      let height = Math.abs(this.currentY - this.startY);

      // Update the rectangle in real time
      this.rectangle = {
        x: x,
        y: y,
        width: width,
        height: height,
      };
    },
    endDrawing() {
      console.log('endDrawing');
      this.drawing = false;
      if (this.rectangle) {
        console.log(`Rectangle recorded: x = ${this.rectangle.x}, y = ${this.rectangle.y}, width = ${this.rectangle.width}, height = ${this.rectangle.height}`);
      }
    },
  },
  props: ["modalContent"],
  components: {}
});
</script>