<template>
  <div class="bg-gray-800 min-h-screen text-white">
    <nav class="bg-gray-900 p-4 flex justify-between items-center">
      <h1 class="">Products</h1>
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
      <h2 class="text-xl mb-4">Products Page</h2>
      <div class="mb-4">
        <input 
          v-model="searchQuery" 
          @input="searchProducts" 
          type="text" 
          placeholder="Search products by title" 
          class="mb-4 p-2 rounded bg-gray-700 text-white w-full">
        <div class="flex items-center mb-4">
          <label for="sort" class="mr-2">Sort by:</label>
          <select v-model="sortCriteria" @change="sortProducts" id="sort" class="p-2 rounded bg-gray-700 text-white mr-4">
            <option value="title">Title</option>
            <option value="price">Price</option>
          </select>
          <label for="order" class="mr-2">Order:</label>
          <select v-model="sortOrder" @change="sortProducts" id="order" class="p-2 rounded bg-gray-700 text-white">
            <option value="asc">Ascending</option>
            <option value="desc">Descending</option>
          </select>
        </div>
      </div>
      <div v-if="filteredProducts.length" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
        <div v-for="product in filteredProducts" :key="product.id" class="bg-gray-700 p-4 rounded">
          <img :src="product.thumbnail" alt="Product Image" class="w-full h-48 object-cover rounded mb-4">
          <h3 class="text-lg font-semibold mb-2">{{ product.title }}</h3>
          <p class="text-sm mb-2">{{ product.description }}</p>
          <p class="text-lg font-bold">{{ product.price }} $</p>
          <button @click="viewDetails(product.id)" class="mt-2 bg-blue-500 hover:bg-blue-400 px-3 py-1 rounded">Details</button>
        </div>
      </div>
      <div v-else>
        <p>No products found</p>
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
      user: null,
      products: [] as Product[],
      searchQuery: '',
      sortCriteria: 'title',
      sortOrder: 'asc',
      filteredProducts: [] as Product[]
    }
  },
  created() {
    this.fetchProducts();
  },
  methods: {
    toggleDropdown() {
      this.dropdownOpen = !this.dropdownOpen;
    },
    async logoutUser() {
      this.$router.push('/');
      console.log('Logged out');
    },
    async fetchProducts() {
      try {
        const response = await fetch('https://dummyjson.com/products');
        const data = await response.json();
        this.products = data.products;
        this.filteredProducts = data.products;
        this.sortProducts();
      } catch (error) {
        console.error('Failed to fetch products:', error);
      }
    },
    searchProducts() {
      const query = this.searchQuery.toLowerCase();
      this.filteredProducts = this.products.filter(product => product.title.toLowerCase().includes(query));
      this.sortProducts();
    },
    sortProducts() {
      const sortKey = this.sortCriteria;
      const order = this.sortOrder === 'asc' ? 1 : -1;
      this.filteredProducts.sort((a, b) => {
        if (a[sortKey] < b[sortKey]) return -1 * order;
        if (a[sortKey] > b[sortKey]) return 1 * order;
        return 0;
      });
    },
    viewDetails(productId: number) {
      this.$router.push(`/products/${productId}`);
    }
  }
})
</script>
