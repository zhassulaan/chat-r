<template>
  <Login v-if="!current_user" @login="login" />
  <div v-else class="main_page">
    <Sidebar :current_user="current_user"
      class="main_page-sidebar"
      @select_chat="select_chat"
      @logout="logout"
    />
    <Window v-if="selected_chat"
      :current_user="current_user"
      :selected_chat="selected_chat"
      :messages="messages"
      class="main_page-window"
      @load_messages="load_messages"
    />
    <img src="@/assets/images/background.png" alt="Background" class="main_page-background" />
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import Login from './components/ChatLogin.vue';
import Sidebar from './components/ChatSidebar.vue';
import Window from './components/ChatWindow.vue';
  
const current_user = ref(localStorage.getItem('current_user') || null);
const selected_chat = ref(null);
const messages = ref([]);

function load_messages() {
  const chatHistory = JSON.parse(localStorage.getItem('chat_messages')) || {};
  const chat_key = [current_user.value, selected_chat.value].sort().join('_');
  messages.value = chatHistory[chat_key] || [];
}
function login(user) {
  current_user.value = user;
  localStorage.setItem('current_user', user);
}
function select_chat(user) {
  selected_chat.value = user;
  load_messages();
}
function logout() {
  current_user.value = null;
  selected_chat.value = null;
  localStorage.removeItem('current_user');
}

onMounted(() => {
  window.addEventListener('storage', load_messages);
});
onBeforeUnmount(() => {
  window.removeEventListener('storage', load_messages);
});
</script>

<style scoped>
.main_page {
  display: flex;
  height: 100vh;
}
.main_page-sidebar,
.main_page-background {
  width: 75%;
}
.main_page-window,
.main_page-background {
  width: 75%;
}
.main_page-background {
  height: 100%;
}
</style>
