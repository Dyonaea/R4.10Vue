<script setup>
import {TrashIcon, HeartIcon} from "@heroicons/vue/24/outline";
import {HeartIcon as filledHeartIcon} from "@heroicons/vue/24/solid";

const props = defineProps({
    post: {
        type: Object,
        required: true,
        
    },
});

const emit = defineEmits(["delete", "like"]);

function deletePost(){
    emit("delete", props.post.id);
}
function like (){
  emit ("like", props.post.id);
}
</script>

<template>
  <article class="card">
    <header>
      <img :src="post.author.avatarUrl" alt="avatar" width="36" height="36" class="avatar"/>
      <a>{{ post.author.username }}</a>
    </header>
    <p>{{ post.content }}</p>

    <footer>
        <button @click="deletePost" class="btn-icon">
            <TrashIcon/>
        </button>
        <button @click="like" class="btn-icon">
          
          <HeartIcon v-if="!post.liked"/>
          <filledHeartIcon v-if="post.liked"/>
        </button>
    </footer>

  </article>
</template>

