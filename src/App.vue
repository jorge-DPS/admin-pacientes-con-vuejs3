<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import Header from "./components/Header.vue";
import Form from "./components/Form.vue";
import Patient from "./components/Patient.vue";
import { uid } from "uid";

const patients = ref([]);
const initialState = {
  id: null,
  name: "",
  owner: "",
  email: "",
  alta: "",
  symptoms: "",
};

const patient = reactive({ ...initialState });

watch(patients, () => {
  saveLocalStorage()
}, {
  deep: true
})

const saveLocalStorage = () => {
  localStorage.setItem('patients', JSON.stringify(patients.value))
}

onMounted(() => {
  const patientsStorage = localStorage.getItem('patients')
  if (patientsStorage) {
    patients.value = JSON.parse(patientsStorage)
  }
})

const savePatient = () => {
  if ( patient.id ) {
    const { id } = patient
    const indexPosition = patients.value.findIndex( (patientState) =>  patientState.id === id) 
    patients.value[indexPosition] = {...patient} 
  }else{
    patients.value.push({
      ...patient,
      id: uid(),
    });
    
  }
  
  Object.assign(patient, initialState);
};

const updatePatient = ( id ) => {
  const patientUpdated = patients.value.find( (patient) => patient.id === id)
  Object.assign(patient, patientUpdated)
  
};

const deletePatient = (id) => {
  if (confirm('¿Deseas eliminar este paciente de forma permanente?')) {
    patients.value = patients.value.filter( (patient) => patient.id !== id )
  }
}
</script>

<template>
  <div class="bg-slate-50 min-h-screen py-10 px-4 sm:px-6 lg:px-8">
    <div class="max-w-7xl mx-auto">
      <Header />

      <div class="mt-12 md:flex gap-10 items-start">
        <!-- Columna Formulario -->
        <div class="md:w-2/5">
          <Form
            v-model:name="patient.name"
            v-model:owner="patient.owner"
            v-model:email="patient.email"
            v-model:alta="patient.alta"
            v-model:symptoms="patient.symptoms"
            @save-patient="savePatient"
            :id="patient.id"
          />
        </div>

        <!-- Columna Listado -->
        <div class="md:w-3/5 mt-12 md:mt-0">
          <h3 class="font-black text-3xl text-center text-slate-800 tracking-tight">
            Seguimiento de <span class="text-indigo-600">Pacientes</span>
          </h3>

          <div v-if="patients.length > 0" class="mt-8">
            <p class="text-lg text-center mb-8 text-slate-500">
              Administra tus
              <span class="text-indigo-600 font-bold">pacientes e historiales</span>
            </p>
            
            <div class="md:h-[750px] overflow-y-auto pr-4 custom-scrollbar space-y-4">
              <Patient
                v-for="patient in patients"
                :key="patient.id"
                :patient="patient"
                @update-patient="updatePatient"
                @delete-patient="deletePatient"
              />
            </div>
          </div>

          <!-- Estado Vacío -->
          <div v-else class="mt-20 p-12 bg-white rounded-2xl shadow-sm border-2 border-dashed border-gray-300 text-center">
            <div class="text-gray-400 mb-4">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197M13 7a4 4 0 11-8 0 4 4 0 018 0z" />
              </svg>
            </div>
            <p class="text-2xl text-gray-400 font-bold uppercase tracking-wider">No hay registros</p>
            <p class="mt-2 text-gray-500">Agrega un paciente para comenzar a gestionarlos</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.custom-scrollbar::-webkit-scrollbar {
  width: 6px;
}
.custom-scrollbar::-webkit-scrollbar-track {
  background: transparent;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
  background-color: #e2e8f0;
  border-radius: 20px;
}
</style>
