<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

// Campos del formulario
const nombre = ref("");
const usuario = ref("");

// Mensaje y lista
const mensaje = ref("");
const clientes = ref([]);

// Cargar lista de clientes
const cargarClientes = async () => {
  try {
    const res = await axios.get("http://localhost:4050/api/cliente");
    clientes.value = res.data;
  } catch (error) {
    console.error("Error al cargar clientes:", error);
  }
};

// Registrar cliente
const registrarCliente = async () => {
  try {
    await axios.post("http://localhost:4050/api/clientes", {
      nombre: nombre.value,
      usuario: usuario.value,
    });

    mensaje.value = "✅ Cliente registrado correctamente";

    nombre.value = "";
    usuario.value = "";

    cargarClientes();
  } catch (error) {
    console.error("❌ Error al registrar:", error);
    mensaje.value = "⚠️ Error al registrar el cliente";
  }
};

// Cargar al montar
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
        <input v-model="usuario" type="email" placeholder="Correo electrónico" required />
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
          <tr v-for="cliente in clientes" :key="cliente.id_cliente">
            <td>{{ cliente.id_cliente }}</td>
            <td>{{ cliente.nombre }}</td>
            <td>{{ cliente.usuario }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>