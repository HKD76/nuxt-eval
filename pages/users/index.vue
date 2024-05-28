<template>
  <div class="bg-gray-800 min-h-screen text-white">
    <nav class="bg-gray-900 p-4 flex justify-between items-center">
      <h1 class="">Users</h1>
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
      <h2 class="text-xl mb-4">Users Page</h2>
      <input 
        v-model="searchQuery" 
        @input="searchUsers" 
        type="text" 
        placeholder="Search users by name" 
        class="mb-4 p-2 rounded bg-gray-700 text-white w-full">
      <div class="flex items-center mb-4">
        <label for="filter" class="mr-2">Filter by Hair Color:</label>
        <select v-model="hairColor" @change="filterUsers" id="filter" class="p-2 rounded bg-gray-700 text-white">
          <option value="">All</option>
          <option value="Brown">Brown</option>
          <option value="Blonde">Blonde</option>
          <option value="Black">Black</option>
          <option value="Red">Red</option>
        </select>
      </div>
      <div v-if="filteredUsers.length" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
        <div v-for="user in filteredUsers" :key="user.id" class="bg-gray-700 p-4 rounded">
          <img :src="user.image" alt="User Image" class="w-full h-48 object-cover rounded mb-4">
          <h3 class="text-lg font-semibold mb-2">{{ user.firstName }} {{ user.lastName }}</h3>
          <p class="text-sm mb-2">{{ user.email }}</p>
          <button @click="viewDetails(user.id)" class="mt-2 bg-blue-500 hover:bg-blue-400 px-3 py-1 rounded">Details</button>
        </div>
      </div>
      <div v-else>
        <p>No users found</p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

interface User {
  id: number;
  firstName: string;
  lastName: string;
  email: string;
  image: string;
}

export default Vue.extend({
  data() {
    return {
      dropdownOpen: false,
      user: null,
      users: [] as User[],
      searchQuery: '',
      hairColor: '',
      filteredUsers: [] as User[]
    }
  },
  created() {
    this.fetchUsers();
  },
  methods: {
    toggleDropdown() {
      this.dropdownOpen = !this.dropdownOpen;
    },
    async logoutUser() {
      this.$router.push('/');
      console.log('Logged out');
    },
    async fetchUsers() {
      try {
        const response = await fetch('https://dummyjson.com/users');
        const data = await response.json();
        this.users = data.users;
        this.filteredUsers = data.users;
      } catch (error) {
        console.error('Failed to fetch users:', error);
      }
    },
    searchUsers() {
      const query = this.searchQuery.toLowerCase();
      this.filteredUsers = this.users.filter(user => {
        const fullName = `${user.firstName} ${user.lastName}`.toLowerCase();
        return fullName.includes(query);
      });
      this.filterUsers();
    },
    async filterUsers() {
      if (this.hairColor === '') {
        this.filteredUsers = this.users.filter(user => {
          const fullName = `${user.firstName} ${user.lastName}`.toLowerCase();
          return fullName.includes(this.searchQuery.toLowerCase());
        });
      } else {
        try {
          const response = await fetch(`https://dummyjson.com/users/filter?key=hair.color&value=${this.hairColor}`);
          const data = await response.json();
          this.filteredUsers = data.users.filter(user => {
            const fullName = `${user.firstName} ${user.lastName}`.toLowerCase();
            return fullName.includes(this.searchQuery.toLowerCase());
          });
        } catch (error) {
          console.error('Failed to fetch filtered users:', error);
        }
      }
    },
    viewDetails(userId: number) {
      this.$router.push(`/users/${userId}`);
    }
  }
})
</script>
