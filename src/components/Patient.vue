<script setup>
defineProps({
  patient: {
    type: Object,
    required: true,
  },
});

defineEmits(["update-patient", "delete-patient"]);
</script>

<template>
  <div class="group mb-6 bg-white shadow-md shadow-slate-200/60 hover:shadow-xl transition-all duration-300 px-8 py-8 rounded-2xl border border-slate-100">
    <div class="flex justify-between items-center mb-6">
      <div>
        <p class="text-[10px] font-black text-indigo-500/60 uppercase tracking-[0.2em] mb-1">Expediente: #{{ patient.id.slice(0, 8) }}</p>
        <h4 class="text-3xl font-black text-slate-800 group-hover:text-indigo-600 transition-colors">{{ patient.name }}</h4>
      </div>
      <div class="text-right">
        <p class="text-[10px] font-bold text-slate-400 uppercase mb-1">Fecha de Alta</p>
        <span class="bg-slate-50 text-slate-700 text-xs font-bold px-4 py-2 rounded-lg border border-slate-200">
          {{ patient.alta }}
        </span>
      </div>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 border-y border-slate-50 py-6">
      <div>
        <p class="text-slate-400 text-[10px] uppercase font-black tracking-widest mb-1">Propietario Responsable</p>
        <p class="font-bold text-slate-700 flex items-center gap-2">
          <span class="w-2 h-2 bg-indigo-400 rounded-full"></span>
          {{ patient.owner }}
        </p>
      </div>

      <div>
        <p class="text-slate-400 text-[10px] uppercase font-black tracking-widest mb-1">Email de Contacto</p>
        <p class="font-bold text-indigo-600 underline underline-offset-4 decoration-indigo-200">{{ patient.email }}</p>
      </div>
    </div>

    <div class="mt-6 p-5 bg-slate-50 rounded-xl border border-slate-100">
      <p class="text-slate-400 text-[10px] uppercase font-black tracking-widest mb-2">Observaciones Médicas</p>
      <p class="text-slate-700 leading-relaxed font-medium italic">
        {{ patient.symptoms }}
      </p>
    </div>

    <div class="flex flex-col sm:flex-row gap-3 mt-8">
      <button
        type="button"
        class="flex-1 py-3 px-6 bg-white border border-slate-200 text-slate-700 hover:bg-slate-50 hover:border-indigo-300 font-bold uppercase rounded-xl transition-all text-xs tracking-widest"
        @click="$emit('update-patient', patient.id)"
      >
        Editar
      </button>

      <button
        type="button"
        class="flex-1 py-3 px-6 bg-rose-50 text-rose-600 hover:bg-rose-600 hover:text-white font-black uppercase rounded-xl transition-all text-xs tracking-widest"
        @click="$emit('delete-patient', patient.id)"
      >
        Eliminar
      </button>
    </div>
  </div>
</template>
