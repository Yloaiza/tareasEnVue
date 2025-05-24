<script setup>
import { ref, computed, onMounted } from 'vue'
import axios from 'axios'
import TaskTable from './components/TaskTable.vue'

const datos = ref([])
const pagActual = ref(1)
const pagTotal = 10
const filtro = ref('todas')
const errorMensaje = ref('')

const obtenerDatos = async () => {
  errorMensaje.value = ''
  try {
    const res = await axios.get('https://jsonplaceholder.typicode.com/todos')
    datos.value = res.data
  } catch (error) {
    console.error('Error al obtener los datos', error)
    errorMensaje.value = '¡Oops! No pudimos cargar los datos. Revisa tu conexión e intenta de nuevo.'
  }
}

onMounted(obtenerDatos)

const datosFiltrados = computed(() => {
  if (filtro.value === 'completadas') {
    return datos.value.filter(d => d.completed)
  } else if (filtro.value === 'pendientes') {
    return datos.value.filter(d => !d.completed)
  }
  return datos.value
})

const totalPaginas = computed(() => Math.ceil(datosFiltrados.value.length / pagTotal))

const datosPaginados = computed(() => {
  const inicio = (pagActual.value - 1) * pagTotal
  return datosFiltrados.value.slice(inicio, inicio + pagTotal)
})

const siguientePagina = () => {
  if (pagActual.value < totalPaginas.value) pagActual.value++
}
const paginaAnterior = () => {
  if (pagActual.value > 1) pagActual.value--
}
</script>

<template>
  <div class="max-w-4xl mx-auto p-4">
    <label for="filtro" class="block mb-2 font-medium">Filtrar:</label>
    <select id="filtro" v-model="filtro" class="mb-4 p-2 border border-gray-300 rounded bg-white text-black shadow-sm">
      <option value="todas">Todas</option>
      <option value="completadas">Completadas</option>
      <option value="pendientes">Pendientes</option>
    </select>

    <div v-if="errorMensaje" class="text-red-500 my-4 font-semibold">
      {{ errorMensaje }}
    </div>

    <TaskTable 
      v-if="!errorMensaje" 
      :datos="datosPaginados" 
      :pagActual="pagActual" 
      :totalPaginas="totalPaginas"
      @pagina-anterior="paginaAnterior"
      @pagina-siguiente="siguientePagina"
    />
  </div>
</template>
