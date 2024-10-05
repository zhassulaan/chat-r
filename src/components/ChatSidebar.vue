<template>
  <div class="sidebar">
    <div class="sidebar-header">
      <span>Logged in as {{ current_user }}</span>
      <Logout @click="logout" />
      <input v-model="search_query" type="text" placeholder="Find chat..." class="sidebar-header-input" />
    </div>

    <List :current_user="current_user"
      :search_query="search_query"
      @select_chat="select_chat"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import List from './ChatList.vue';
import Logout from './icons/Logout.vue';

const current_user = ref(localStorage.getItem('current_user') || null);
const search_query = ref('');
const emit = defineEmits(['select_chat', 'logout']);

function select_chat(user) {
  emit('select_chat', user);
}
function logout() {
  emit('logout');
}
</script>

<style scoped>
.sidebar {
  background-color: var(--clr-white);
}
.sidebar-header {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 4px;
  padding: 8px 12px;
}
.sidebar-header-input {
  width: 100%;
  height: 42px;
  min-height: 42px;
  max-height: 42px;
  padding: 12px 16px;
  border: 1px solid var(--input-search-border-color);
  border-radius: 22px;
  background-color: var(--input-search-background-color);
  transition: 0s border-color;
  line-height: var(--line-height-16);
}
</style>
