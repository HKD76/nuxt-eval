<template>
  <div class="bg-gray-800 min-h-screen text-white">
    <nav class="bg-gray-900 p-4 flex justify-between items-center">
      <h1 class="">User Details</h1>
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
      <h2 class="text-xl mb-4">User Details</h2>
      <div v-if="user" class="bg-gray-700 p-4 rounded">
        <div class="w-full h-64 mb-4 overflow-hidden rounded">
          <img :src="user.image" alt="User Image" class="w-full h-full object-cover">
        </div>
        <h3 class="text-lg font-semibold mb-2">{{ user.firstName }} {{ user.lastName }}</h3>
        <p class="text-sm mb-2"><strong>Maiden Name:</strong> {{ user.maidenName }}</p>
        <p class="text-sm mb-2"><strong>Age:</strong> {{ user.age }}</p>
        <p class="text-sm mb-2"><strong>Gender:</strong> {{ user.gender }}</p>
        <p class="text-sm mb-2"><strong>Email:</strong> {{ user.email }}</p>
        <p class="text-sm mb-2"><strong>Phone:</strong> {{ user.phone }}</p>
        <p class="text-sm mb-2"><strong>Username:</strong> {{ user.username }}</p>
        <p class="text-sm mb-2"><strong>Birth Date:</strong> {{ user.birthDate }}</p>
        <p class="text-sm mb-2"><strong>Blood Group:</strong> {{ user.bloodGroup }}</p>
        <p class="text-sm mb-2"><strong>Height:</strong> {{ user.height }} cm</p>
        <p class="text-sm mb-2"><strong>Weight:</strong> {{ user.weight }} kg</p>
        <p class="text-sm mb-2"><strong>Eye Color:</strong> {{ user.eyeColor }}</p>
        <p class="text-sm mb-2"><strong>Hair:</strong> {{ user.hair.color }} - {{ user.hair.type }}</p>
        <p class="text-sm mb-2"><strong>IP Address:</strong> {{ user.ip }}</p>
        <p class="text-sm mb-2"><strong>Role:</strong> {{ user.role }}</p>
        <div class="mt-4">
          <h4 class="text-lg font-semibold">Address</h4>
          <p class="text-sm mb-2">{{ user.address.address }}, {{ user.address.city }}, {{ user.address.state }}, {{ user.address.country }} - {{ user.address.postalCode }}</p>
        </div>
        <div class="mt-4">
          <h4 class="text-lg font-semibold">Company</h4>
          <p class="text-sm mb-2"><strong>Department:</strong> {{ user.company.department }}</p>
          <p class="text-sm mb-2"><strong>Title:</strong> {{ user.company.title }}</p>
          <p class="text-sm mb-2"><strong>Company:</strong> {{ user.company.name }}</p>
          <p class="text-sm mb-2"><strong>Company Address:</strong> {{ user.company.address.address }}, {{ user.company.address.city }}, {{ user.company.address.state }}, {{ user.company.address.country }} - {{ user.company.address.postalCode }}</p>
        </div>
        <div class="mt-4">
          <h4 class="text-lg font-semibold">Bank</h4>
          <p class="text-sm mb-2"><strong>Card Number:</strong> {{ user.bank.cardNumber }}</p>
          <p class="text-sm mb-2"><strong>Card Type:</strong> {{ user.bank.cardType }}</p>
          <p class="text-sm mb-2"><strong>Card Expire:</strong> {{ user.bank.cardExpire }}</p>
          <p class="text-sm mb-2"><strong>Currency:</strong> {{ user.bank.currency }}</p>
          <p class="text-sm mb-2"><strong>IBAN:</strong> {{ user.bank.iban }}</p>
        </div>
        <div class="mt-4">
          <h4 class="text-lg font-semibold">Crypto</h4>
          <p class="text-sm mb-2"><strong>Coin:</strong> {{ user.crypto.coin }}</p>
          <p class="text-sm mb-2"><strong>Wallet:</strong> {{ user.crypto.wallet }}</p>
          <p class="text-sm mb-2"><strong>Network:</strong> {{ user.crypto.network }}</p>
        </div>
        <button @click="goBack" class="mt-4 bg-gray-500 hover:bg-gray-400 px-3 py-1 rounded">Back to Users</button>
        <button @click="deleteUser" class="mt-4 bg-red-500 hover:bg-red-400 px-3 py-1 rounded ml-2">Delete User</button>
        
        <!-- Console Simulation Box -->
        <div v-if="response" class="mt-4 p-4 bg-gray-900 rounded shadow">
          <p class="text-green-500"><strong>Status:</strong> {{ response.status }}</p>
          <pre class="text-white bg-gray-800 p-2 rounded" v-html="response.data"></pre>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

interface Address {
  address: string;
  city: string;
  state: string;
  postalCode: string;
  country: string;
}

interface CompanyAddress {
  address: string;
  city: string;
  state: string;
  postalCode: string;
  country: string;
}

interface Hair {
  color: string;
  type: string;
}

interface Bank {
  cardExpire: string;
  cardNumber: string;
  cardType: string;
  currency: string;
  iban: string;
}

interface Company {
  department: string;
  name: string;
  title: string;
  address: CompanyAddress;
}

interface Crypto {
  coin: string;
  wallet: string;
  network: string;
}

interface User {
  id: number;
  firstName: string;
  lastName: string;
  maidenName: string;
  age: number;
  gender: string;
  email: string;
  phone: string;
  username: string;
  birthDate: string;
  image: string;
  bloodGroup: string;
  height: number;
  weight: number;
  eyeColor: string;
  hair: Hair;
  ip: string;
  address: Address;
  macAddress: string;
  university: string;
  bank: Bank;
  company: Company;
  ein: string;
  ssn: string;
  userAgent: string;
  crypto: Crypto;
  role: string;
}

export default Vue.extend({
  data() {
    return {
      dropdownOpen: false,
      user: null as User | null,
      response: null as { status: number, data: string } | null
    };
  },
  async asyncData({ params }) {
    const response = await fetch(`https://dummyjson.com/users/${params.id}`);
    const user = await response.json();
    return { user };
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
      this.$router.push('/users');
    },
    async deleteUser() {
      if (!this.user) return;

      try {
        const response = await fetch(`https://dummyjson.com/users/${this.user.id}`, {
          method: 'DELETE',
        });
        const data = await response.json();
        
        const formattedData = JSON.stringify(data, null, 2).replace(
          /"isDeleted": true/,
          '"isDeleted": <span class="text-red-400">true</span>'
        );

        this.response = {
          status: response.status,
          data: formattedData
        };
      } catch (error) {
        console.error('Error:', error);
      }
    }
  }
});
</script>
