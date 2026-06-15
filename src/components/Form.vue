<script setup>
import { computed, reactive } from "vue";
import Alert from "./Alert.vue";

const alert = reactive({
  type: "",
  message: "",
});

const emit = defineEmits([
  "update:name",
  "update:owner",
  "update:email",
  "update:alta",
  "update:symptoms",
  "save-patient",
]);

const props = defineProps({
  id: {
    type: [String, null],
    required: true
  },
  name: {
    type: String,
    required: true,
  },
  owner: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  alta: {
    type: String,
    required: true,
  },
  symptoms: {
    type: String,
    required: true,
  },
});

const validate = (e) => {
  if (Object.values(props).includes("")) {
    alert.message = "Todos los campos con obligatorios";
    alert.type = "error";
    return;
  }

  emit("save-patient");
  alert.message = 'Pasiente alamcenado correctamente'
  alert.type = 'exito'

  setTimeout(() => {
    Object.assign(alert, {
      type:'',
      message:''
    })
  }, 3000)
};

const editing = computed(() => {
  return props.id
})
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>

    <p class="text-lg mt-5 text-center mb-10">
      Añade pacientes y
      <span class="text-indigo-600 font-bold">Administralos</span>
    </p>

    <Alert v-if="alert.message" :alert="alert" />

    <form
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validate"
    >
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold">
          Nombre Mascota
        </label>
        <input
          id="mascota"
          type="text"
          placeholder="Nombre de la mascota"
          class="border w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="name"
          @input="$emit('update:name', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label
          for="propietario"
          class="block text-gray-700 uppercase font-bold"
        >
          Nombre propietario
        </label>
        <input
          id="propietario"
          type="text"
          placeholder="Nombre del propietario"
          class="border w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="owner"
          @input="$emit('update:owner', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold">
          Correo
        </label>
        <input
          id="email"
          type="email"
          placeholder="Email del propietario"
          class="border w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold">
          Alta
        </label>
        <input
          id="alta"
          type="date"
          class="border-2 w-full mt-2 placeholder-gray-400 rounded-md"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold">
          Sintomas
        </label>
        <textarea
          id="sintomas"
          placeholder="Describe los sintomas del paciente"
          class="border w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
          :value="symptoms"
          @input="$emit('update:symptoms', $event.target.value)"
        />
      </div>

      <input
        type="submit"
        class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-400 cursor-pointer transition-colors"
        :value="[editing ? 'Guardar Cambios' : 'Registrar paciente']"
      />
    </form>
  </div>
</template>
