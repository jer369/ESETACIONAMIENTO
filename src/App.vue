<template>
  <div id="app">
    <EmpleadoForm @registrar="agregarEmpleado" />

    <div class="filtros">
      <select v-model="filtroVehiculo" @change="filtrarEmpleados">
        <option value="">Todos los vehículos</option>
        <option value="Automóvil">Automóvil</option>
        <option value="Moto">Moto</option>
      </select>
      <input 
        type="text" 
        v-model="busquedaNombre" 
        @input="buscarEmpleado" 
        placeholder="Buscar por nombre" 
      />
    </div>

    <TablaEmpleados 
      :empleados="empleadosFiltrados" 
      @editar="editarEmpleado" 
    />
  </div>
</template>

<script>
import EmpleadoForm from './components/EmpleadoForm.vue';
import TablaEmpleados from './components/TablaEmpleados.vue';
import axios from 'axios';

export default {
  components: {
    EmpleadoForm,
    TablaEmpleados
  },
  data() {
    return {
      empleados: [],
      empleadosFiltrados: [],
      filtroVehiculo: '',
      busquedaNombre: ''
    };
  },
  created() {
    this.obtenerEmpleados();
  },
  methods: {
    async obtenerEmpleados() {
      try {
        const response = await axios.get('http://localhost:3000/empleados');
        this.empleados = response.data;
        this.empleadosFiltrados = response.data;
      } catch (error) {
        console.error("Hubo un error al obtener los empleados:", error);
      }
    },
    agregarEmpleado(nuevoEmpleado) {
      this.empleados.push(nuevoEmpleado);
      this.empleadosFiltrados = this.empleados;
    },
    editarEmpleado(empleadoEditado) {
      const index = this.empleados.findIndex(e => e.vehiculo.placa === empleadoEditado.vehiculo.placa);
      if (index !== -1) {
        this.empleados.splice(index, 1, empleadoEditado);
        this.empleadosFiltrados = this.empleados;
      }
    },
    filtrarEmpleados() {
      this.empleadosFiltrados = this.empleados.filter(empleado => 
        this.filtroVehiculo ? empleado.vehiculo.tipo === this.filtroVehiculo : true
      );
    },
    buscarEmpleado() {
      const nombreLower = this.busquedaNombre.toLowerCase();
      this.empleadosFiltrados = this.empleados.filter(empleado => 
        empleado.nombre.toLowerCase().includes(nombreLower)
      );
    }
  }
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  padding: 20px;
}
.filtros {
  margin-bottom: 20px;
  display: flex;
  gap: 10px;
}
select, input {
  padding: 8px;
  font-size: 16px;
}
</style>
