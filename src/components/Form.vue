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
  alert.message = 'Paciente almacenado correctamente'
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
  <div class="bg-white shadow-xl shadow-slate-200/50 rounded-3xl p-8 border border-slate-100">
    <h2 class="font-black text-3xl text-center text-slate-800 tracking-tight">Registro de Citas</h2>

    <p class="text-lg mt-2 text-center mb-8 text-slate-500">
      Añade pacientes y
      <span class="text-indigo-600 font-bold">gestiónalos</span>
    </p>

    <Alert v-if="alert.message" :alert="alert" />

    <form
      class="space-y-5"
      @submit.prevent="validate"
    >
      <div class="mb-5">
        <label for="mascota" class="block text-slate-600 uppercase font-bold text-xs tracking-widest">
          Nombre Mascota
        </label>
        <input
          id="mascota"
          type="text"
          placeholder="Nombre de la mascota"
          class="border border-slate-200 w-full p-3 mt-2 placeholder-slate-400 rounded-xl focus:border-indigo-500 focus:ring-4 focus:ring-indigo-100/50 outline-none transition-all bg-slate-50/30"
          :value="name"
          @input="$emit('update:name', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label
          for="propietario"
          class="block text-slate-600 uppercase font-bold text-xs tracking-widest"
        >
          Nombre propietario
        </label>
        <input
          id="propietario"
          type="text"
          placeholder="Nombre del propietario"
          class="border border-slate-200 w-full p-3 mt-2 placeholder-slate-400 rounded-xl focus:border-indigo-500 focus:ring-4 focus:ring-indigo-100/50 outline-none transition-all bg-slate-50/30"
          :value="owner"
          @input="$emit('update:owner', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="email" class="block text-slate-600 uppercase font-bold text-xs tracking-widest">
          Correo
        </label>
        <input
          id="email"
          type="email"
          placeholder="Email del propietario"
          class="border border-slate-200 w-full p-3 mt-2 placeholder-slate-400 rounded-xl focus:border-indigo-500 focus:ring-4 focus:ring-indigo-100/50 outline-none transition-all bg-slate-50/30"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="alta" class="block text-slate-600 uppercase font-bold text-xs tracking-widest">
          Alta
        </label>
        <input
          id="alta"
          type="date"
          class="border border-slate-200 w-full p-3 mt-2 placeholder-slate-400 rounded-xl focus:border-indigo-500 focus:ring-4 focus:ring-indigo-100/50 outline-none transition-all bg-slate-50/30 text-slate-600"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="sintomas" class="block text-slate-600 uppercase font-bold text-xs tracking-widest">
          Sintomas
        </label>
        <textarea
          id="sintomas"
          placeholder="Describe los sintomas del paciente"
          class="border border-slate-200 w-full p-3 mt-2 placeholder-slate-400 rounded-xl h-32 focus:border-indigo-500 focus:ring-4 focus:ring-indigo-100/50 outline-none transition-all resize-none bg-slate-50/30"
          :value="symptoms"
          @input="$emit('update:symptoms', $event.target.value)"
        />
      </div>

      <input
        type="submit"
        class="bg-indigo-600 w-full p-4 text-white uppercase font-black hover:bg-indigo-700 cursor-pointer transition-all rounded-xl shadow-lg shadow-indigo-200 active:scale-[0.98]"
        :value="[editing ? 'Guardar Cambios' : 'Registrar paciente']"
      />
    </form>
  </div>
</template>
