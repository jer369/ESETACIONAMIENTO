<template>
  <div>
    <h2>Empleados Registrados</h2>
    <table class="tabla">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Área</th>
          <th>Tipo de Vehículo</th>
          <th>Número de Placa</th>
          <th>Color</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(empleado, index) in empleados" :key="index">
          <td>{{ empleado.nombre }}</td>
          <td>{{ empleado.area }}</td>
          <td>{{ empleado.vehiculo.tipo }}</td>
          <td>{{ empleado.vehiculo.placa }}</td>
          <td>{{ empleado.vehiculo.color }}</td>
          <td>
            <button @click="habilitarEdicion(empleado)">Editar</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="empleadoEnEdicion" class="edicion-form">
      <h3>Editar Empleado</h3>
      <form @submit.prevent="guardarEdicion">
        <input type="text" v-model="empleadoEnEdicion.nombre" placeholder="Nombre" required />
        <select v-model="empleadoEnEdicion.area">
          <option value="producción">Producción</option>
          <option value="finanzas">Finanzas</option>
          <option value="contabilidad">Contabilidad</option>
        </select>
        <input type="text" v-model="empleadoEnEdicion.vehiculo.tipo" placeholder="Tipo de Vehículo" required />
        <input type="text" v-model="empleadoEnEdicion.vehiculo.placa" placeholder="Placa" required />
        <input type="text" v-model="empleadoEnEdicion.vehiculo.color" placeholder="Color" required />
        <button type="submit">Guardar</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    empleados: Array
  },
  data() {
    return {
      empleadoEnEdicion: null
    };
  },
  methods: {
    habilitarEdicion(empleado) {
      this.empleadoEnEdicion = { ...empleado };
    },
    guardarEdicion() {
      this.$emit('editar', this.empleadoEnEdicion);
      this.empleadoEnEdicion = null;
    }
  }
};
</script>

<style scoped>
.tabla {
  width: 100%;
  margin: 20px auto;
  border-collapse: collapse;
}

.tabla th, .tabla td {
  padding: 12px;
  text-align: left;
}

.tabla th {
  background-color: #4CAF50;
  color: white;
}

.tabla td {
  background-color: #f2f2f2;
}

.tabla tr:nth-child(even) {
  background-color: #ddd;
}

.edicion-form {
  margin-top: 20px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
}
</style>