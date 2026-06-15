<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import Header from "./components/Header.vue";
import Form from "./components/Form.vue";
import Patient from "./components/Patient.vue";
import { uid } from "uid";

const patients = ref([]);

const patient = reactive({
  id: null,
  name: "",
  owner: "",
  email: "",
  alta: "",
  symptoms: "",
});

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
  
  Object.assign(patient, {
    id: null,
    name: "",
    owner: "",
    email: "",
    alta: "",
    symptoms: "",
  });
};

const updatePatient = ( id ) => {
  const patientUpdated = patients.value.filter( (patient) => patient.id === id)[0]
  Object.assign(patient, patientUpdated)
  
};

const deletePatient = (id) => {
  console.log(id);
  
  patients.value = patients.value.filter( (patient) => patient.id !== id )
}
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />

    <div class="mt-12 md:flex">
      <Form
        v-model:name="patient.name"
        v-model:owner="patient.owner"
        v-model:email="patient.email"
        v-model:alta="patient.alta"
        v-model:symptoms="patient.symptoms"
        @save-patient="savePatient"
        :id="patient.id"
      />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Adminstra tus pacientes</h3>
        <div v-if="patients.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de
            <span class="text-indigo-600 font-bold">pacientes </span>
          </p>
          <Patient
            v-for="patient in patients"
            :patient="patient"
            @update-patient="updatePatient"
            @delete-patient="deletePatient"
          />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
