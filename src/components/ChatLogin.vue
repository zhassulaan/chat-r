<template>
  <div class="login">
    <img src="@/assets/images/logo.png" alt="Logo" class="login-icon" />
    <h4 class="login-title">Log in to Chat'r app</h4>
    <span class="login-text">Select the user from the list</span>

    <div class="login-buttons">
      <button v-for="user in users"
        :key="user"
        class="login-buttons-item"
        @click="select_user(user)"
      >
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

<style scoped lang="sass">
@mixin jcc
  display: flex
  justify-content: center
.login
  flex-direction: column
  align-items: center
  width: 100%
  height: 100%
  background-color: var(--clr-white)
  color: var(--clr-black)
  @include jcc
  &-icon
    width: 196px
    height: 196px
  &-title
    margin: 22px 0 14px
  &-text
    margin: 1rem auto 2rem
  &-buttons
    overflow-y: scroll
    border-radius: 4px
    height: 210px
    padding: 6px
    background-color: var(--background-color)
    &-item
      @include jcc
      border: none
      border-radius: 8px
      transition: background-color .3s
      margin: 3% 0
      width: 210px
      padding: 10px 0
      background-color: var(--primary-color)
      cursor: pointer
      color: var(--clr-white)
      &:hover
        background-color: var(--dark-primary-color)
</style>
