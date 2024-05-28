<template>
  <div class="bg-gray-800 min-h-screen text-white">
    <nav class="bg-gray-900 p-4 flex justify-between items-center">
      <h1 class="">PAGE INDEX</h1>
      <div>
        <span v-if="user" class="text-green-500 mr-4">✓</span>
        <button v-if="user" @click="toggleDropdown" class="mr-4 bg-gray-700 hover:bg-gray-600 px-4 py-2 rounded">
          Menu
        </button>
        <div v-if="dropdownOpen && user" class="absolute bg-gray-700 text-white mt-2 rounded shadow-lg">
          <a class="block px-4 py-2 hover:bg-gray-600" href="/products">Products</a>
          <a class="block px-4 py-2 hover:bg-gray-600" href="/carts">Carts</a>
          <a class="block px-4 py-2 hover:bg-gray-600" href="/recipes">Recipes</a>
          <a class="block px-4 py-2 hover:bg-gray-600" href="/users">Users</a>
          <a class="block px-4 py-2 hover:bg-gray-600" href="/posts">Posts</a>
          <a class="block px-4 py-2 hover:bg-gray-600" href="/comments">Comments</a>
          <a class="block px-4 py-2 hover:bg-gray-600" href="/todos">Todos</a>
          <a class="block px-4 py-2 hover:bg-gray-600" href="/quotes">Quotes</a>
          <a class="block px-4 py-2 hover:bg-gray-600" href="/crud">CRUD</a>
        </div>
        <button v-if="!user" @click="registerUser" class="mr-4 bg-gray-700 hover:bg-gray-600 px-4 py-2 rounded">Register</button>
        <button v-if="!user" @click="loginUser" class="mr-4 bg-blue-700 hover:bg-blue-600 px-4 py-2 rounded">Login</button>
        <button v-if="user" @click="logoutUser" class="bg-red-700 hover:bg-red-600 px-4 py-2 rounded">Logout</button>
      </div>
    </nav>
    <div class="p-4">
      <div class="mb-4">
        <input v-model="username" type="text" placeholder="Username" class="bg-gray-700 hover:bg-gray-600 px-4 py-2 rounded text-white">
      </div>
      <div class="mb-4">
        <input v-model="password" type="password" placeholder="Password" class="bg-gray-700 hover:bg-gray-600 px-4 py-2 rounded text-white">
      </div>
      <button @click="loginUser" class="bg-blue-700 hover:bg-blue-600 px-4 py-2 rounded">Login</button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  data() {
    return {
      username: '',
      password: '',
      token: '',
      user: null,
      dropdownOpen: false
    }
  },
  methods: {
    toggleDropdown() {
      this.dropdownOpen = !this.dropdownOpen;
    },
    async registerUser() {
    },
    async loginUser() {
      try {
        const response = await fetch('https://dummyjson.com/auth/login', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            username: this.username,
            password: this.password,
            expiresInMins: 30
          })
        });
        const data = await response.json();
        if (data.token) {
          this.user = data;
          this.token = data.token;
          //@ts-ignore
          this.$router.push(`/${this.user.id}`);
        } else {
          console.error('Erreur de connexion: token non reçu');
        }
      } catch (error) {
        console.error(error);
      }
    },
    async logoutUser() {
      this.user = null;
      this.token = '';
      this.$router.push('/');
      console.log('Logged out');
    },
    async getCurrentUser() {
      try {
        const response = await fetch('https://dummyjson.com/auth/me', {
          method: 'GET',
          headers: {
            'Authorization': `Bearer ${this.token}`
          }
        });
        const userData = await response.json();
        console.log(userData);
      } catch (error) {
        console.error(error);
      }
    },
    async refreshSession() {
      try {
        const response = await fetch('https://dummyjson.com/auth/refresh', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${this.token}`
          },
          body: JSON.stringify({
            expiresInMins: 30
          })
        });
        const data = await response.json();
        this.token = data.token;
        console.log(data);
      } catch (error) {
        console.error(error);
      }
    }
  }
})
</script>
