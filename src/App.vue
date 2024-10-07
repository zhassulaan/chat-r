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

function login(user) {
  current_user.value = user;
  localStorage.setItem('current_user', user);
}
function logout() {
  current_user.value = null;
  selected_chat.value = null;
  localStorage.removeItem('current_user');
}
function load_messages() {
  const chat_history = JSON.parse(localStorage.getItem('chat_messages')) || {};
  const chat_key = [current_user.value, selected_chat.value].sort().join('_');
  messages.value = (chat_history[chat_key] || []).slice().reverse();
}
function select_chat(user) {
  selected_chat.value = user;
  load_messages();
}

onMounted(() => {
  window.addEventListener('storage', load_messages);
});
onBeforeUnmount(() => {
  window.removeEventListener('storage', load_messages);
});
</script>

<style scoped lang="scss">
.main_page {
  display: flex;
  height: 100vh;
  &-sidebar {
    width: 25%;
  }
  &-window,
  &-background {
    width: 75%;
  }
  &-background {
    height: 100%;
  }
}
</style>
