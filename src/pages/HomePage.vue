<script setup>
import { ref, computed } from "vue";
import PostCard from "@/components/PostCard.vue";

const text = ref("booh");
const trimmedText = computed(() => text.value.trim());
const posts = ref([]);
const sortedPost = computed(() => [...posts.value].sort((a, b) => b.createdAt - a.createdAt));
function addPost() {
  const newPost = {
    id: Math.random().toString(36).substring(2),
    content: trimmedText.value,
    createdAt: new Date(),
    author: {
      username: "poubelles",
      avatarUrl: "https://media1.tenor.com/m/2GwvVXLsJ4QAAAAd/trash-cans-dancing-trash-cans.gif",
      //avatarUrl: "https://statics.koreanbuilds.net/tile_200x200/Pyke.webp"
    },
    liked: false,
  };
  posts.value.push(newPost);
  text.value = "";
}

const apiPosts = ref([]);
function fetchPosts() {
  const result = fetch("https://posts-crud-api.vercel.app/posts");
  result
    .then((reponse) => reponse.json())
    .then((data) => {
      apiPosts.value = data;
    });
}
function deletePost(id) {
  apiPosts.value = apiPosts.value.filter((post) => post.id !== id);
}
function like(id) {
  apiPosts.value.find((post) => post.id == id).liked ^= true;
}
fetchPosts();
</script>

<template>
  <main>
    <div class="container">
      <form class="card" @submit.prevent="addPost">
        <textarea action="post" id="post" placeholder="Qui de .. feur" v-model="text"></textarea>
        <button type="submit" :disabled="!trimmedText">Publier</button>
      </form>

      <h2 v-if="!apiPosts.length">Aucun post pour le moment.</h2>
      <PostCard
        v-for="(post, index) in apiPosts"
        :key="index"
        :post="post"
        @delete="deletePost"
        @like="like"
      />
    </div>
  </main>
</template>
