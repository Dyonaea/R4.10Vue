<script setup>
import { ref, computed } from "vue";
import { useRouter } from "vue-router";
const router = useRouter();

const registering = ref(false);

const nom = ref("");
const email = ref("");
const mdp = ref("");
const trimmed = computed(() => nom.value.trim() && email.value.trim() && mdp.value.trim());
function register() {
  registering.value = true;
//   console.log(registering.value);

//   const newUser = {
//     username: nom.value,
//     email: email.value,
//     password: mdp.value,
//   };

//   console.log(newUser);
//   setTimeout(() => {
//     router.push({ name: "home" });
//   }, 1000);

  
  fetch("https://posts-crud-api.vercel.app/register", {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        
    },
    body: JSON.stringify({
        username: nom.value,
        email: email.value,
        password: mdp.value
    })
  })
    .then((response) => response.json())
    .then((data) => {
        console.log(data);
        localStorage.setItem("user", JSON.stringify(data));
        router.push({ name: "home" });
    });
}
</script>

<template>
  <main>
    <div class="container">
      <form class="card" @submit.prevent="register">
        <h1 style="margin-bottom: 1rem">Créer un compte</h1>
        <input action="post" id="nom" placeholder="Nom" v-model="nom" />
        <input action="post" id="email" placeholder="Email" v-model="email" />
        <input action="post" id="mdp" placeholder="Mot de Passe" v-model="mdp" type="password" />
        <button type="submit" :disabled="registering || !trimmed" :class="{ loading: registering }">register</button>
      </form>
    </div>
  </main>
</template>

<style scoped>
input {
  background: var(--color-bg-tertiary);
  border: none;
  border-radius: 10px;
  color: var(--color-text-primary);
  outline: none;
  padding: 1rem;
  margin-bottom: 1rem;
}
button {
  width: 100%;
}
</style>