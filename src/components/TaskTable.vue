<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  datos: Array,
  pagActual: Number,
  totalPaginas: Number
})

const emit = defineEmits(['pagina-anterior', 'pagina-siguiente'])

const anterior = () => emit('pagina-anterior')
const siguiente = () => emit('pagina-siguiente')
</script>

<template>
  <div class="overflow-x-auto">
    <table class="w-full border border-gray-200 shadow-md rounded-lg overflow-hidden">
      <thead class="bg-blue-500 text-black">
        <tr>
          <th class="py-2 px-4 text-left">USER ID</th>
          <th class="py-2 px-4 text-left">ID</th>
          <th class="py-2 px-4 text-left">Título</th>
          <th class="py-2 px-4 text-left">Estado</th>
        </tr>
      </thead>
      <tbody>
        <tr 
          v-for="dato in datos" 
          :key="dato.id" 
          class=" hover:bg-gray-950 "
        >
          <td class="py-2 px-4">{{ dato.userId }}</td>
          <td class="py-2 px-4">{{ dato.id }}</td>
          <td class="py-2 px-4">{{ dato.title }}</td>
          <td class="py-2 px-4">
            <span :class="dato.completed ? 'text-green-600' : 'text-yellow-600'">
              {{ dato.completed ? '✅ Completada' : '⏳ Pendiente' }}
            </span>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="flex justify-between items-center mt-4">
      <button 
        @click="anterior" 
        :disabled="pagActual === 1" 
        class="px-4 py-2 bg-blue-500 hover:bg-blue-600 text-white font-semibold rounded disabled:opacity-50"
      >
        Anterior
      </button>
      <span class="font-medium">Página {{ pagActual }} de {{ totalPaginas }}</span>
      <button 
        @click="siguiente" 
        :disabled="pagActual === totalPaginas" 
        class="px-4 py-2 bg-blue-500 hover:bg-blue-600 text-white font-semibold rounded disabled:opacity-50"
      >
        Siguiente
      </button>
    </div>
  </div>
</template>
