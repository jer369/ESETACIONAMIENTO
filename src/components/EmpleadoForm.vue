<template>
    <div class="form-container">
      <h2 class="form-title">Registrar Empleado y Vehículo</h2>
      <form @submit.prevent="registrarEmpleado" class="formulario">
        <div class="campo">
          <label for="nombre" class="label">Nombre del Empleado:</label>
          <input
            type="text"
            id="nombre"
            v-model="empleado.nombre"
            required
            class="input"
            :class="{'input-error': errores.nombre}"
          />
          <span v-if="errores.nombre" class="error">{{ errores.nombre }}</span>
        </div>
  
        <div class="campo">
          <label for="area" class="label">Área:</label>
          <select id="area" v-model="empleado.area" required class="input">
            <option value="producción">Producción</option>
            <option value="finanzas">Finanzas</option>
            <option value="contabilidad">Contabilidad</option>
          </select>
        </div>
  
        <div class="campo">
          <label for="tipoVehiculo" class="label">Tipo de Vehículo:</label>
          <select id="tipoVehiculo" v-model="vehiculo.tipo" required class="input">
            <option value="auto">Automóvil</option>
            <option value="moto">Moto</option>
          </select>
        </div>
  
        <div class="campo">
          <label for="placa" class="label">Número de Placa:</label>
          <input
            type="text"
            id="placa"
            v-model="vehiculo.placa"
            required
            class="input"
            :class="{'input-error': errores.placa}"
          />
          <span v-if="errores.placa" class="error">{{ errores.placa }}</span>
        </div>
  
        <div class="campo">
          <label for="color" class="label">Color del Vehículo:</label>
          <input
            type="text"
            id="color"
            v-model="vehiculo.color"
            required
            class="input"
            :class="{'input-error': errores.color}"
          />
          <span v-if="errores.color" class="error">{{ errores.color }}</span>
        </div>
  
        <button type="submit" class="submit-button">Registrar</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        empleado: {
          nombre: '',
          area: ''
        },
        vehiculo: {
          tipo: '',
          placa: '',
          color: ''
        },
        errores: {
          nombre: '',
          placa: '',
          color: ''
        }
      };
    },
    methods: {
      async registrarEmpleado() {
       
        this.errores = { nombre: '', placa: '', color: '' };
        let esValido = true;
  
        if (this.empleado.nombre.length < 3) {
          this.errores.nombre = "El nombre debe tener al menos 3 caracteres.";
          esValido = false;
        }
  
        const placaRegex = /^[A-Za-z0-9]{6,}$/;
        if (!placaRegex.test(this.vehiculo.placa)) {
          this.errores.placa = "La placa debe tener al menos 6 caracteres alfanuméricos.";
          esValido = false;
        }
  
        if (!this.vehiculo.color) {
          this.errores.color = "El color del vehículo es obligatorio.";
          esValido = false;
        }
  
        if (esValido) {
          const nuevoEmpleado = {
            nombre: this.empleado.nombre,
            area: this.empleado.area,
            vehiculo: {
              tipo: this.vehiculo.tipo,
              placa: this.vehiculo.placa,
              color: this.vehiculo.color
            }
          };
  
          try {
           
            await axios.post('http://localhost:3000/empleados', nuevoEmpleado);
            this.$emit('registrar', nuevoEmpleado);
            
            this.empleado.nombre = '';
            this.empleado.area = '';
            this.vehiculo.tipo = '';
            this.vehiculo.placa = '';
            this.vehiculo.color = '';
          } catch (error) {
            console.error("Hubo un error al registrar el empleado:", error);
          }
        }
      }
    }
  };
  </script>
  
  <style scoped>
.form-container {
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.form-title {
  text-align: center;
  font-size: 24px;
  color: #333;
  margin-bottom: 20px;
}

.formulario {
  display: flex;
  flex-direction: column;
}

.campo {
  margin-bottom: 15px;
}

.label {
  font-weight: bold;
  margin-bottom: 8px;
  display: inline-block;
  color: #333;
}

.input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
  box-sizing: border-box;
}

.input-error {
  border-color: #e74c3c;
}

.error {
  color: #e74c3c;
  font-size: 12px;
}

.submit-button {
  background-color: #28a745; 
  color: white;
  padding: 12px;
  font-size: 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.submit-button:hover {
  background-color: #218838; 
}

.submit-button:active {
  background-color: #1e7e34; 
}
</style>
