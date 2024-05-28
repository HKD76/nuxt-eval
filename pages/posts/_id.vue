<template>
  <div class="bg-gray-800 min-h-screen text-white">
    <nav class="bg-gray-900 p-4 flex justify-between items-center">
      <h1 class="">Post Details</h1>
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
      <h2 class="text-xl mb-4">Post Details</h2>
      <div v-if="post" class="bg-gray-700 p-4 rounded">
        <h3 class="text-lg font-semibold mb-2">{{ post.title }}</h3>
        <p class="text-sm mb-2">{{ post.body }}</p>
        <p class="text-sm mb-2"><strong>User ID:</strong> {{ post.userId }}</p>
        <div class="mt-4">
          <h4 class="text-lg font-semibold">Tags</h4>
          <ul>
            <li v-for="tag in post.tags" :key="tag">{{ tag }}</li>
          </ul>
        </div>
        <div class="mt-4">
          <h4 class="text-lg font-semibold">Reactions</h4>
          <p class="text-sm mb-2"><strong>Likes:</strong> {{ post.reactions.likes }}</p>
          <p class="text-sm mb-2"><strong>Dislikes:</strong> {{ post.reactions.dislikes }}</p>
        </div>
        <p class="text-sm mb-2"><strong>Views:</strong> {{ post.views }}</p>
      </div>
      <div v-if="comments && comments.length" class="mt-4">
        <h3 class="text-lg font-semibold mb-2">Comments</h3>
        <ul>
          <li v-for="comment in comments" :key="comment.id" class="bg-gray-700 p-2 rounded mb-2">
            <p class="text-sm"><strong>Username:</strong> {{ comment.user.username }}</p>
            <p class="text-sm"><strong>Comment:</strong> {{ comment.body }}</p>
          </li>
        </ul>
      </div>
      <div v-else>
        <p>No comments found for this post</p>
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
  reactions: { likes: number; dislikes: number };
  views: number;
  userId: number;
}

interface Comment {
  id: number;
  userId: number;
  body: string;
}

export default Vue.extend({
  data() {
    return {
      dropdownOpen: false,
      post: null as Post | null,
      comments: [] as Comment[]
    };
  },
  async asyncData({ params }) {
    try {
      const [postResponse, commentsResponse] = await Promise.all([
        fetch(`https://dummyjson.com/posts/${params.id}`),
        fetch(`https://dummyjson.com/comments/post/${params.id}`)
      ]);
      const postData = await postResponse.json();
      const commentsData = await commentsResponse.json();
      return { post: postData, comments: commentsData.comments };
    } catch (error) {
      console.error('Failed to fetch post and comments:', error);
      return { post: null, comments: [] };
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
});
</script>
