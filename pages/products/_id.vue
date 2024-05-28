<template>
    <div class="bg-gray-800 min-h-screen text-white">
      <nav class="bg-gray-900 p-4 flex justify-between items-center">
        <h1 class="">Product Details</h1>
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
        <h2 class="text-xl mb-4">Product Details</h2>
        <div v-if="product" class="bg-gray-700 p-4 rounded">
          <div class="w-full h-64 mb-4 overflow-hidden rounded">
            <img :src="product.thumbnail" alt="Product Image" class="w-full h-full object-contain">
          </div>
          <h3 class="text-lg font-semibold mb-2">{{ product.title }}</h3>
          <p class="text-sm mb-2">{{ product.description }}</p>
          <p class="text-lg font-bold">{{ product.price }} $</p>
          <button @click="goBack" class="mt-4 bg-gray-500 hover:bg-gray-400 px-3 py-1 rounded">Back to Products</button>
        </div>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import Vue from 'vue'
  
  interface Product {
    id: number;
    title: string;
    description: string;
    price: number;
    thumbnail: string;
  }
  
  export default Vue.extend({
    data() {
      return {
        dropdownOpen: false,
        product: null as Product | null
      }
    },
    async asyncData({ params }) {
      const response = await fetch(`https://dummyjson.com/products/${params.id}`);
      const product = await response.json();
      return { product };
    },
    methods: {
      toggleDropdown() {
        this.dropdownOpen = !this.dropdownOpen;
      },
      async logoutUser() {
        this.$router.push('/');
        console.log('Logged out');
      },
      goBack() {
        this.$router.push('/products');
      }
    }
  })
  </script>
  