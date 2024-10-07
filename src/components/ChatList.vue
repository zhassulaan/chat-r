<template>
  <ul class="chat_list">
    <li v-for="user in filtered_users"
      :key="user"
      class="chat_list-item"
      @click="select_chat(user)"
    >
      {{ user }}
    </li>
  </ul>
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

<style scoped lang="sass">
.chat_list
  background-color: var(--background-color)
  &-item
    transition: background-color 0.3s
    padding: 16px 20px
    cursor: pointer
    &:hover
      background-color: var(--dark-background-color)
</style>
