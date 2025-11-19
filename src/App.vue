<template>
  <div class="min-h-screen bg-gray-50">
    <header class="bg-blue-900 text-white shadow-lg">
      <div class="container mx-auto px-4 py-6">
        <h1 class="text-3xl font-bold">Sistema de Registro de Requerimientos</h1>
        <p class="text-blue-200 mt-2">Interseguro</p>
      </div>
    </header>

    <main class="container mx-auto px-4 py-8">
      <div class="mb-6">
        <button
          @click="showForm = true"
          class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-6 rounded-lg shadow-md transition duration-200"
        >
          + Nuevo Requerimiento
        </button>
      </div>

      <RegistroRequerimiento 
        v-if="showForm"
        @cerrar="showForm = false"
        @guardar="agregarRequerimiento"
      />

      <ListaRequerimientos 
        :requerimientos="requerimientos"
        @eliminar="eliminarRequerimiento"
      />
    </main>
  </div>
</template>

<script>
import { ref } from 'vue'
import RegistroRequerimiento from './components/RegistroRequerimiento.vue'
import ListaRequerimientos from './components/ListaRequerimientos.vue'

export default {
  name: 'App',
  components: {
    RegistroRequerimiento,
    ListaRequerimientos
  },
  setup() {
    const showForm = ref(false)
    const requerimientos = ref([])

    const agregarRequerimiento = (requerimiento) => {
      requerimientos.value.unshift({
        id: Date.now(),
        ...requerimiento,
        fechaRegistro: new Date().toLocaleString('es-ES')
      })
      showForm.value = false
    }

    const eliminarRequerimiento = (id) => {
      requerimientos.value = requerimientos.value.filter(req => req.id !== id)
    }

    return {
      showForm,
      requerimientos,
      agregarRequerimiento,
      eliminarRequerimiento
    }
  }
}
</script>

