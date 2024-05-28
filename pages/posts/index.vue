<template>
  <div class="bg-gray-800 min-h-screen text-white">
    <nav class="bg-gray-900 p-4 flex justify-between items-center">
      <h1 class="">Posts</h1>
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
      <h2 class="text-xl mb-4">Posts</h2>
      <div v-if="posts.length" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
        <div v-for="post in posts" :key="post.id" class="bg-gray-700 p-4 rounded">
          <h3 class="text-lg font-semibold mb-2">{{ post.title }}</h3>
          <p class="text-sm mb-2">{{ post.body }}</p>
          <p class="text-sm mb-2"><strong>Tags:</strong> {{ post.tags.join(', ') }}</p>
          <div class="flex items-center mb-2">
            <img src="../../static/png-transparent-love-hearts-love-hearts-red-heart-love-heart-computer-icons-thumbnail.png" alt="Like" class="w-4 h-4 mr-1 text-green-500">
            <p class="text-sm">Likes: <span class="text-green-500">{{ post.reactions.likes }}</span></p>
          </div>
          <div class="flex items-center mb-2">
            <img src="../../static/png-transparent-broken-heart-broken-heart-heartbreak-love-heart-silhouette-thumbnail.png" alt="Dislike" class="w-4 h-4 mr-1 text-red-500">
            <p class="text-sm">Dislikes: <span class="text-red-500">{{ post.reactions.dislikes }}</span></p>
          </div>
          <p class="text-sm mb-2"><strong>Views:</strong> {{ post.views }}</p>
          <p class="text-sm mb-2"><strong>User ID:</strong> {{ post.userId }}</p>
          <button @click="viewPostDetails(post.id)" class="bg-blue-500 hover:bg-blue-400 px-4 py-2 rounded">View Details</button>
        </div>
      </div>
      <div v-else>
        <p>No posts found</p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

interface Post {
  id: number;
  title: string;
  body: string;
  tags: string[];
  reactions: {
    likes: number;
    dislikes: number;
  };
  views: number;
  userId: number;
}

export default Vue.extend({
  data() {
    return {
      dropdownOpen: false,
      posts: [] as Post[]
    };
  },
  async created() {
    try {
      const response = await fetch('https://dummyjson.com/posts');
      const data = await response.json();
      this.posts = data.posts;
    } catch (error) {
      console.error('Failed to fetch posts:', error);
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
    viewPostDetails(postId: number) {
      this.$router.push(`/posts/${postId}`);
    }
  }
});
</script>
