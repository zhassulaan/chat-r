<template>
  <div class="login">
    <img src="@/assets/images/logo.png" alt="Logo" class="login-icon" />
    <h4 class="login-title">Log in to Chat'r app</h4>
    <span class="login-text">Select the user from the list</span>

    <div class="login-buttons">
      <button v-for="user in users" :key="user" class="login-button" @click="select_user(user)">
        {{ user }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const users = ref(['User1', 'User2', 'User3', 'User4', 'User5']);
const emit = defineEmits(['login']);

function select_user(user) {
  emit('login', user);
};
</script>

<style scoped>
.login {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  background-color: var(--clr-white);
  color: var(--clr-black);
}
.login-icon {
  width: 196px;
  height: 196px;
}
.login-title {
  margin: 22px 0 14px;
}
.login-text {
  margin: 1rem auto 2rem;
}
.login-buttons {
  height: 210px;
  background-color: var(--background-color);
  overflow: scroll;
  padding: 6px;
  border-radius: 4px;
}
.login-buttons::-webkit-scrollbar {
  display: none;
}
.login-button {
  display: flex;
  justify-content: center;
  width: 210px;
  background-color: var(--primary-color);
  color: #FFF;
  border: none;
  border-radius: 8px;
  padding: 10px 0;
  margin: 3% 0;
  cursor: pointer;
  transition: background-color .3s;
}
.login-button:hover {
  background-color: var(--dark-primary-color);
}
</style>
