<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

// Campos del formulario
const nombre = ref("");
const usuario = ref("");

// Mensaje y lista
const mensaje = ref("");
const clientes = ref([]);

// URL base del backend desplegado
const API_URL = "https://backend01-2-psq6.onrender.com/api/clientes";

// Cargar lista de clientes
const cargarClientes = async () => {
  try {
    const res = await axios.get(API_URL);
    clientes.value = res.data;
  } catch (error) {
    console.error("Error al cargar clientes:", error);
    mensaje.value = "⚠ Error al cargar los clientes";
  }
};

// Registrar cliente
const registrarCliente = async () => {
  try {
    if (!nombre.value || !usuario.value) {
      mensaje.value = "⚠ Completa todos los campos";
      return;
    }

    await axios.post(API_URL, {
      nombre: nombre.value,
      usuario: usuario.value,
    });

    mensaje.value = "✅ Cliente registrado correctamente";

    // Limpiar campos
    nombre.value = "";
    usuario.value = "";

    // Recargar lista
    cargarClientes();
  } catch (error) {
    console.error("❌ Error al registrar:", error);
    mensaje.value = "⚠ Error al registrar el cliente";
  }
};

// Ejecutar al montar
onMounted(() => {
  cargarClientes();
});
</script>

<template>
  <div class="contenedor">
    <h1>Gestión de Clientes</h1>

    <!-- Formulario -->
    <div class="form-container">
      <h2>Registrar Cliente</h2>
      <form @submit.prevent="registrarCliente" class="formulario">
        <input v-model="nombre" type="text" placeholder="Nombre completo" required />
        <input v-model="usuario" type="text" placeholder="Usuario" required />
        <button type="submit">Registrar</button>
      </form>

      <p v-if="mensaje" class="mensaje">{{ mensaje }}</p>
    </div>

    <!-- Lista -->
    <div class="lista-clientes">
      <h2>Clientes Registrados</h2>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Usuario</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="cliente in clientes" :key="cliente.id">
            <td>{{ cliente.id }}</td>
            <td>{{ cliente.nombre }}</td>
            <td>{{ cliente.usuario }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.contenedor {
  max-width: 800px;
  margin: 30px auto;
  font-family: "Segoe UI", sans-serif;
  padding: 20px;
  border-radius: 15px;
  background-color: #fdfdfd;
  box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
}

h2 {
  color: #42b883;
  margin-bottom: 15px;
}

.formulario {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
}

button {
  padding: 10px;
  background-color: #42b883;
  border: none;
  color: white;
  font-weight: bold;
  border-radius: 8px;
  cursor: pointer;
}

button:hover {
  background-color: #2c9e70;
}

.mensaje {
  margin-top: 10px;
  font-weight: bold;
  color: #2c9e70;
}

.lista-clientes {
  margin-top: 30px;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th {
  background-color: #42b883;
  color: rgb(0, 0, 0);
  padding: 10px;
  text-align: left;
}

td {
  border-bottom: 1px solid #000000;
  padding: 8px;
}

tr:hover {
  background-color: #d9f9e4;
}
</style>
