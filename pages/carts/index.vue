<template>
  <div class="bg-gray-800 min-h-screen text-white">
    <nav class="bg-gray-900 p-4 flex justify-between items-center">
      <h1 class="">Carts</h1>
      <div class="relative">
        <button @click="toggleDropdown" class="mr-4 bg-gray-700 hover:bg-gray-600 px-4 py-2 rounded">
          Menu
        </button>
       <div v-if="dropdownOpen" class="absolute bg-gray-700 text-white mt-2 rounded shadow-lg">
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
        <button @click="logoutUser" class="bg-red-700 hover:bg-red-600 px-4 py-2 rounded">Logout</button>
      </div>
    </nav>
    <div class="p-4">
      <h2 class="text-xl">Carts Page</h2>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  data() {
    return {
      dropdownOpen: false,
      user: null
    }
  },
  methods: {
    toggleDropdown() {
      this.dropdownOpen = !this.dropdownOpen;
    },
    async logoutUser() {
      this.$router.push('/');
      console.log('Logged out');
    }
  }
})
</script>
