<template>
  <div class="chat_list">
    <ul>
      <li v-for="user in filtered_users" :key="user" @click="select_chat(user)">
        {{ user }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const users = ref(['User1', 'User2', 'User3', 'User4', 'User5']);
const props = defineProps({
  current_user: String,
  search_query: String,
});
const emit = defineEmits(['select_chat']);
const filtered_users = computed(() => {
  return users.value.filter(user => 
  user !== props.current_user &&
  user.toLowerCase().includes(props.search_query.toLowerCase())
  );
});

function select_chat(user) {
  if (user !== props.current_user)
    emit('select_chat', user);
};
</script>

<style scoped>
.chat_list {
  background-color: #F6F6F6;
}
.chat_list li {
  padding: 16px 20px;
  cursor: pointer;
  transition: background-color 0.3s;
}
.chat_list li:hover {
  background-color: #DCDCDC;
}
</style>
