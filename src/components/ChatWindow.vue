<template>
  <div class="chat_window">
    <span class="chat_window-header">Chat with <b>{{ selected_chat }}</b></span>

    <div class="chat_window-messages">
      <div
        class="chat_window-messages-item"
        v-for="message in messages"
        :key="message.timestamp"
        :class="{
          'chat_window-messages-item_right': message.from === current_user,
          'chat_window-messages-item_left': message.from !== current_user
        }"
      >
        <p>{{ message.text }}</p>
      </div>
    </div>

    <div class="chat_window-controller">
      <input v-model="new_message" placeholder="Type your message..."  class="chat_window-controller-input"/>
      <button @click="send_message" class="chat_window-controller-button">↑</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  current_user: String,
  selected_chat: String,
  messages: Array,
});
const new_message = ref('');
const emit = defineEmits(['load_messages']);

function save_message(message) {
  let chat_history = JSON.parse(localStorage.getItem('chat_messages')) || {};
  const chat_key = [props.current_user, props.selected_chat].sort().join('_');
  if (!chat_history[chat_key])
    chat_history[chat_key] = [];
  chat_history[chat_key].push(message);
  localStorage.setItem('chat_messages', JSON.stringify(chat_history));
  emit('load_messages');
}
function send_message() {
  const message = {
    from: props.current_user,
    to: props.selected_chat,
    text: new_message.value,
    timestamp: new Date().toISOString(),
  };
  if (new_message.value !== '') {
    save_message(message);
    new_message.value = '';
  }
}
</script>

<style scoped lang="sass">
.chat_window
  position: absolute
  right: 0
  height: 100vh
  &-header,
  &-messages,
  &-controller
    position: inherit
  &-header
    z-index: 2
    border-left: 1px solid var(--clr-grey)
    width: 100%
    height: 56px
    padding: 20px
    background-color: var(--clr-white)
  &-messages,
  &-controller
    left: 50%
    display: flex
    flex-direction: column-reverse 
    transform: translate(-50%)
    width: 60%
  &-messages
    bottom: 64px
    max-height: calc(100vh - 120px)
    overflow-y: scroll
    &-item
      min-width: 60%
      max-width: 60%
      padding: 10px
      margin: 5px
      border-radius: 10px
      &_right
        margin-left: auto
        background-color: var(--primary-chat-color)
        text-align: right
      &_left
        margin-right: auto
        background-color: var(--secondary-chat-color)
        text-align: left
  &-controller
    bottom: 0
    overflow: hidden
    border-radius: 18px
    background-color: var(--clr-white)
    &-input
      border: 0
      width: 100%
      padding: 12px 42px 12px 10px
    &-button
      position: absolute
      top: 50%
      right: 5px
      transform: translateY(-50%)
      transition: background-color 0.3s
      border: none
      border-radius: 50%
      width: 32px
      height: 32px
      background-color: var(--primary-color)
      cursor: pointer
      color: var(--clr-white)
      &:hover
        background-color: var(--dark-primary-color)
</style>
