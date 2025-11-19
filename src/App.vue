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
          @click="abrirFormularioNuevo"
          class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-6 rounded-lg shadow-md transition duration-200"
        >
          + Nuevo Requerimiento
        </button>
      </div>

      <RegistroRequerimiento 
        v-if="showForm"
        :requerimiento-editar="requerimientoEditar"
        @cerrar="cerrarFormulario"
        @guardar="guardarRequerimiento"
      />

      <ListaRequerimientos 
        :requerimientos="requerimientos"
        @eliminar="eliminarRequerimiento"
        @editar="editarRequerimiento"
      />

      <!-- Sistema de notificaciones -->
      <div class="fixed bottom-4 right-4 z-50 space-y-2">
        <transition-group name="notification" tag="div">
          <div
            v-for="notificacion in notificaciones"
            :key="notificacion.id"
            :class="[
              'px-6 py-4 rounded-lg shadow-lg min-w-[300px] max-w-md',
              notificacion.tipo === 'exito' 
                ? 'bg-green-500 text-white' 
                : 'bg-red-500 text-white'
            ]"
          >
            <div class="flex items-center justify-between">
              <p class="font-semibold">{{ notificacion.mensaje }}</p>
              <button
                @click="cerrarNotificacion(notificacion.id)"
                class="ml-4 text-white hover:text-gray-200"
              >
                ×
              </button>
            </div>
          </div>
        </transition-group>
      </div>
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
    const requerimientoEditar = ref(null)
    const notificaciones = ref([])

    const agregarRequerimiento = (requerimiento) => {
      requerimientos.value.unshift({
        id: Date.now(),
        ...requerimiento,
        fechaRegistro: new Date().toLocaleString('es-ES')
      })
      showForm.value = false
      mostrarNotificacion('Requerimiento creado exitosamente', 'exito')
    }

    const actualizarRequerimiento = (requerimientoActualizado) => {
      try {
        const index = requerimientos.value.findIndex(req => req.id === requerimientoActualizado.id)
        if (index !== -1) {
          // Mantener la fecha de registro original
          requerimientos.value[index] = {
            ...requerimientoActualizado,
            fechaRegistro: requerimientos.value[index].fechaRegistro
          }
          mostrarNotificacion('Requerimiento actualizado exitosamente', 'exito')
        } else {
          throw new Error('Requerimiento no encontrado')
        }
      } catch (error) {
        mostrarNotificacion('Error al actualizar el requerimiento: ' + error.message, 'error')
        return // No cerrar el formulario en caso de error
      }
      cerrarFormulario()
    }

    const guardarRequerimiento = (requerimiento) => {
      if (requerimiento.id) {
        actualizarRequerimiento(requerimiento)
      } else {
        agregarRequerimiento(requerimiento)
      }
    }

    const abrirFormularioNuevo = () => {
      requerimientoEditar.value = null
      showForm.value = true
    }

    const editarRequerimiento = (requerimiento) => {
      requerimientoEditar.value = { ...requerimiento }
      showForm.value = true
    }

    const cerrarFormulario = () => {
      showForm.value = false
      requerimientoEditar.value = null
    }

    const eliminarRequerimiento = (id) => {
      requerimientos.value = requerimientos.value.filter(req => req.id !== id)
    }

    const mostrarNotificacion = (mensaje, tipo = 'exito') => {
      const id = Date.now()
      notificaciones.value.push({ id, mensaje, tipo })
      
      // Auto-cerrar después de 5 segundos
      setTimeout(() => {
        cerrarNotificacion(id)
      }, 5000)
    }

    const cerrarNotificacion = (id) => {
      const index = notificaciones.value.findIndex(n => n.id === id)
      if (index !== -1) {
        notificaciones.value.splice(index, 1)
      }
    }

    return {
      showForm,
      requerimientos,
      requerimientoEditar,
      notificaciones,
      abrirFormularioNuevo,
      agregarRequerimiento,
      actualizarRequerimiento,
      guardarRequerimiento,
      editarRequerimiento,
      cerrarFormulario,
      eliminarRequerimiento,
      mostrarNotificacion,
      cerrarNotificacion
    }
  }
}
</script>

