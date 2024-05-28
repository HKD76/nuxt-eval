<template>
  <div class="bg-gray-800 min-h-screen text-white">
    <nav class="bg-gray-900 p-4 flex justify-between items-center">
      <h1 class="">Comments</h1>
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
      <h2 class="text-xl mb-4">Comments</h2>
      <div v-if="comments.length" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
        <div v-for="comment in comments" :key="comment.id" class="bg-gray-700 p-4 rounded">
          <h3 class="text-lg font-semibold mb-2">{{ comment.user.username }}</h3>
          <p class="text-sm mb-2">{{ comment.body }}</p>
          <p class="text-sm mb-2"><strong>Email:</strong> {{ comment.email }}</p>
          <p class="text-sm mb-2"><strong>Post ID:</strong> {{ comment.postId }}</p>
          <button @click="viewPost(comment.postId)" class="bg-blue-500 hover:bg-blue-400 px-4 py-2 rounded">View Post</button>
        </div>
      </div>
      <div v-else>
        <p>No comments found</p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

interface Comment {
  id: number;
  name: string;
  email: string;
  body: string;
  postId: number;
}

export default Vue.extend({
  data() {
    return {
      dropdownOpen: false,
      comments: [] as Comment[]
    };
  },
  async created() {
    try {
      const response = await fetch('https://dummyjson.com/comments');
      const data = await response.json();
      this.comments = data.comments;
    } catch (error) {
      console.error('Failed to fetch comments:', error);
    }
  },
  methods: {
    toggleDropdown() {
      this.dropdownOpen = !this.dropdownOpen;
    },
    async logoutUser() {
      this.$router.push('/');
      console.log('Logged out');
    },
    viewPost(postId: number) {
      this.$router.push(`/posts/${postId}`);
    }
  }
});
</script>
