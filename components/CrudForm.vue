<template>
  <div class="bg-gray-800 min-h-screen text-white">
    <nav class="bg-gray-900 p-4 flex justify-between items-center">
      <h1 class="">CRUD Form</h1>
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
      <h2 class="text-xl mb-4">Add New</h2>
      <div class="mb-4">
        <label for="entityType" class="mr-2">Select Type:</label>
        <select v-model="entityType" @change="resetForm" id="entityType" class="p-2 rounded bg-gray-700 text-white">
          <option value="user">User</option>
          <option value="product">Product</option>
        </select>
      </div>
      <form @submit.prevent="handleSubmit">
        <div v-if="entityType === 'user'">
          <div class="mb-4">
            <label for="firstName" class="block">First Name:</label>
            <input v-model="formData.firstName" type="text" id="firstName" class="p-2 rounded bg-gray-700 text-white w-full">
          </div>
          <div class="mb-4">
            <label for="lastName" class="block">Last Name:</label>
            <input v-model="formData.lastName" type="text" id="lastName" class="p-2 rounded bg-gray-700 text-white w-full">
          </div>
          <div class="mb-4">
            <label for="age" class="block">Age:</label>
            <input v-model="formData.age" type="number" id="age" class="p-2 rounded bg-gray-700 text-white w-full">
          </div>
          <!-- Add other user fields here -->
        </div>
        <div v-if="entityType === 'product'">
          <div class="mb-4">
            <label for="title" class="block">Title:</label>
            <input v-model="formData.title" type="text" id="title" class="p-2 rounded bg-gray-700 text-white w-full">
          </div>
          <div class="mb-4">
            <label for="price" class="block">Price:</label>
            <input v-model="formData.price" type="number" id="price" class="p-2 rounded bg-gray-700 text-white w-full">
          </div>
          <div class="mb-4">
            <label for="description" class="block">Description:</label>
            <textarea v-model="formData.description" id="description" class="p-2 rounded bg-gray-700 text-white w-full"></textarea>
          </div>
          <!-- Add other product fields here -->
        </div>
        <button type="submit" class="bg-blue-500 hover:bg-blue-400 px-4 py-2 rounded">Submit</button>
      </form>
      
      <!-- Console Simulation Box -->
      <div v-if="response" class="mt-4 p-4 bg-gray-900 rounded shadow">
        <p class="text-green-500"><strong>Status:</strong> {{ response.status }}</p>
        <pre class="text-white bg-black p-2 rounded">{{ response.data }}</pre>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  data() {
    return {
      dropdownOpen: false,
      entityType: 'user',
      formData: {
        firstName: '',
        lastName: '',
        age: null,
        title: '',
        price: null,
        description: ''
      },
      response: null as { status: number, data: string } | null
    };
  },
  methods: {
    toggleDropdown() {
      this.dropdownOpen = !this.dropdownOpen;
    },
    resetForm() {
      this.formData = {
        firstName: '',
        lastName: '',
        age: null,
        title: '',
        price: null,
        description: ''
      };
      this.response = null;
    },
    async handleSubmit() {
      const url =
        this.entityType === 'user'
          ? 'https://dummyjson.com/users/add'
          : 'https://dummyjson.com/products/add';

      try {
        const response = await fetch(url, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(this.formData)
        });
        const data = await response.json();
        this.response = {
          status: response.status,
          data: JSON.stringify(data, null, 2)
        };
      } catch (error) {
        console.error('Error:', error);
      }
    },
    async logoutUser() {
      this.$router.push('/');
      console.log('Logged out');
    }
  }
});
</script>
