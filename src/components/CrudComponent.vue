<template>
    <div>
      <h2>CRUD de Usuarios</h2>
      
      <!-- Formulario para crear/editar usuarios -->
      <form @submit.prevent="saveUser">
        <input v-model="user.name" placeholder="Nombre" required>
        <input v-model="user.email" placeholder="Email" required>
        <button type="submit">{{ editing ? 'Actualizar' : 'Crear' }}</button>
      </form>
  
      <!-- Lista de usuarios -->
      <ul>
        <li v-for="user in users" :key="user.id">
          {{ user.name }} ({{ user.email }})
          <button @click="editUser(user)">Editar</button>
          <button @click="deleteUser(user.id)">Eliminar</button>
        </li>
      </ul>
  
      <!-- Mensaje de error -->
      <p v-if="errorMessage" style="color: red;">{{ errorMessage }}</p>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        users: [],
        user: { id: null, name: '', email: '' },
        editing: false,
        errorMessage: ''
      }
    },
    methods: {
      async fetchUsers() {
        try {
          const response = await axios.get('https://jsonplaceholder.typicode.com/users');
          this.users = response.data;
        } catch (error) {
          this.errorMessage = 'Error al cargar usuario/s: ' + error.message;
        }
      },
      async saveUser() {
        try {
          if (this.editing) {
            // Simular actualización local
            const index = this.users.findIndex(u => u.id === this.user.id);
            if (index !== -1) {
              this.users[index] = { ...this.user };
              this.errorMessage = 'Usuario actualizado exitosamente';
            }
          } else {
            // Simular creación local
            const newUser = { ...this.user, id: Date.now() }; // ID temporal
            this.users.push(newUser);
            this.errorMessage = 'Usuario creado exitosamente';
          }
          this.user = { id: null, name: '', email: '' };
          this.editing = false;
        } catch (error) {
          this.errorMessage = 'Error al guardar usuario: ' + error.message;
        }
      },
      editUser(user) {
        this.user = { ...user };
        this.editing = true;
      },
      async deleteUser(id) {
        try {
          // Simular eliminación local
          this.users = this.users.filter(user => user.id !== id);
          this.errorMessage = 'Usuario eliminado localmente (simulado)';
        } catch (error) {
          this.errorMessage = 'Error al eliminar usuario: ' + error.message;
        }
      }
    },
    mounted() {
      this.fetchUsers();
    }
  }
  </script>