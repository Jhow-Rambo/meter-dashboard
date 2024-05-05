<template>
	<div :class="`modal z-50 fixed w-full h-full top-0 left-0 flex items-center justify-center`">
		<div @click="changeModalValue" class="absolute w-full h-full bg-gray-900 opacity-50 modal-overlay"></div>

		<div class="z-50 w-11/12 mx-auto overflow-y-auto bg-white rounded shadow-lg modal-container md:max-w-md">
			<div class="px-6 py-4 text-left modal-content">
				<div class="flex items-center justify-between pb-3">
					<p class="text-2xl font-bold">Adicionar Medidor</p>
					<div class="z-50 cursor-pointer modal-close" @click="changeModalValue">
						<svg class="text-black fill-current" xmlns="http://www.w3.org/2000/svg" width="18" height="18"
							viewBox="0 0 18 18">
							<path
								d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z" />
						</svg>
					</div>
				</div>
				<form>
					<div class="mb-6">
						<label for="exampleFormControlInput1"
							class="inline-block mb-2 text-gray-700 form-label">Nome</label>
						<input 
							type="text" 
              v-model="meterForm.name"
							class="form-control block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0
						focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" 
							id="exampleFormControlInput1" 
							placeholder="Medidor..." />
					</div>

					<div class="mb-6">
						<label for="exampleFormControlInput1"
							class="inline-block mb-2 text-gray-700 form-label">Localização</label>
						<input v-model="meterForm.location" type="text" class="
						form-control
						block
						w-full
						px-3
						py-1.5
						text-base
						font-normal
						text-gray-700
						bg-white bg-clip-padding
						border border-solid border-gray-300
						rounded
						transition
						ease-in-out
						m-0
						focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none
						" id="exampleFormControlInput1" placeholder="Parque Tecnologico de Itaipu - Setor 001..." />
					</div>

					<div>
						<h3 class="inline-block mb-2 text-gray-700">Imagem</h3>
						<div class="flex items-center justify-center w-full mb-5">
							<label v-if="!image" for="dropzone-file"
								class="flex flex-col items-center justify-center w-full h-64 border-2 border-dashed rounded-lg cursor-pointer border-white-300 bg-white-50 dark:hover:bg-bray-800 dark:bg-white-700 hover:bg-white-100 dark:border-white-600 dark:hover:border-white-500 dark:hover:bg-white-600">
								<div class="flex flex-col items-center justify-center pt-5 pb-6">
									<svg aria-hidden="true" class="w-10 h-10 mb-3 text-gray-400" fill="none"
										stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
										<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
											d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12">
										</path>
									</svg>
									<p class="mb-2 text-sm text-gray-500 dark:text-gray-400">
										<span class="font-semibold">Clique para fazer o upload</span> ou arraste e solte
									</p>
									<p class="text-xs text-gray-500 dark:text-gray-400">SVG, PNG, JPG or GIF (MAX.
										800x400px)</p>
								</div>
								<input id="dropzone-file" type="file" class="hidden" @change="handleFileUpload"
									ref="fileInput" />
							</label>

							<div v-if="image">
								<img :src="image" alt="Uploaded Image" />
							</div>
						</div>
					</div>


					<button type="button" data-mdb-ripple="true" data-mdb-ripple-color="light"
						class="inline-block px-6 py-2.5 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out"
						@click="saveMeter">
						Adicionar
					</button>
				</form>

			</div>
		</div>
	</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import api from "@/api/api";
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

export default defineComponent({
  data() {
    return {
        image: '' as string | ArrayBuffer | null,
        meterForm: {
            name: "",
            location: "",
            meter_img: ""
        }
    };
  },
    emits: ["closeModal", "updateData"],
  methods: {
		changeModalValue() {
			this.$emit("closeModal");
    },
    saveMeter() {
        api.post('/meters', this.meterForm)
            .then((response: { data: any; }) => {
                toast("Medidor Salvo com Sucesso!", {
                    theme: "auto",
                    type: "success",
                     
                    autoClose: 5000,
                });

                this.$emit("updateData");
                this.changeModalValue();
            })
            .catch((error: any) => {
                toast("Erro ao Salvar Medidor!", {
                    theme: "auto",
                    type: "error",
                     
                    autoClose: 5000,
                });
            });
    },
    handleFileUpload(event: { target: { files: any[]; }; }) {
        const file = event.target.files[0];
        const reader = new FileReader();

        reader.onload = (e) => {
            if (e.target) {
                this.meterForm.meter_img = e.target.result as string;

                this.image = e.target.result;
            }
        };

        reader.readAsDataURL(file);
    },
	},
	props: ["modalContent"]
});
</script>
