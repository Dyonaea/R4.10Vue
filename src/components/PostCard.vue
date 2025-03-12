<script setup>
import {TrashIcon, HeartIcon} from "@heroicons/vue/24/outline";
import {HeartIcon as filledHeartIcon} from "@heroicons/vue/24/solid";
import {useRouter} from "vue-router";
const router = useRouter();


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

function gotoUserP(){
  router.push({name: "user", params: {username: props.post.author.username}});
}
</script>

<template>
  <article class="card">
    <header>
      <img :src="post.author.avatarUrl" alt="avatar" width="36" height="36" class="avatar"/>
      <a @click="gotoUserP">{{ post.author.username }}</a>
    </header>
    <p>{{ post.content }}</p>

    <footer>
      <button @click="like" class="btn-icon">
        

        <component :is="post.liked ? filledHeartIcon : HeartIcon" :class="{active: post.liked}"/>
      </button>
        <button @click="deletePost" class="btn-icon">
            <TrashIcon/>
        </button>
    </footer>

  </article>
</template>

