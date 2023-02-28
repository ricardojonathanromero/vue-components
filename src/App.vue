<script setup>
import {onMounted, ref} from 'vue';
import BlogPost from '@/components/BlogPost.vue';
import PaginationPost from '@/components/PaginationPost.vue';
import LoadingSpinner from '@/components/LoadingSpinner.vue';

const posts = ref([]);
const favPost = ref({});
const currentPage = ref(0);
const limit = 10;
const offer = ref(limit);
const loading = ref(true);

const configureFav = post => favPost.value = post;

const getPosts = async () => {
  let data;
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts');
    data = res.status !== 200 ? [] : await res.json();
  } catch (e) {
    console.error('error response from service:', e);
    data = [];
  } finally {
    setTimeout(() => {
      loading.value = false;
    }, 2000)
  }

  posts.value = data;
};

const nextPage = () => {
  currentPage.value += limit;
  offer.value += limit;
};

const previousPage = () => {
  currentPage.value -= limit;
  offer.value -= limit;
}

onMounted(() => {
  getPosts();
});
</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>
    <template v-if="favPost.hasOwnProperty('id')">
      <h2>My Favorite Post:</h2>
      <BlogPost :id="favPost.id" :title="favPost.title" :body="favPost.body" class="mb-3" />
    </template>
    <h2 v-else>
      Posts:
    </h2>
    <br>
    <PaginationPost
      class="mb-3"
      :disabledPrev="currentPage <= 0"
      :disabledNext="posts.length - offer < limit"
      @previousPage="previousPage"
      @nextPage="nextPage"
    />
    <BlogPost
      v-for="post in posts.slice(currentPage, offer)"
      :key="post.id"
      :id="post.id"
      :title="post.title"
      :body="post.body"
      @changeFav="configureFav"
      class="mb-3"
    />
  </div>
</template>
