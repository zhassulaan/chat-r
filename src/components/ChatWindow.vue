<template>
  <div class="chat_window">
    <span class="chat_window-header">Chat with <b>{{ selected_chat }}</b></span>

    <div class="chat_window-messages">
      <div
        class="chat_window-message"
        v-for="message in messages"
        :key="message.timestamp"
        :class="{
          'chat_window-message_right': message.from === current_user,
          'chat_window-message_left': message.from !== current_user
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
  save_message(message);
  new_message.value = '';
}
</script>

<style scoped lang="scss">
.chat_window {
  position: absolute;
  right: 0;
  height: 100vh;
  &-header,
  &-messages,
  &-controller {
    position: inherit;
  }
  &-header {
    border-left: 1px solid var(--clr-grey);
    width: 100%;
    padding: 20px;
    background-color: var(--clr-white);
  }
  &-messages,
  &-controller {
    left: 50%;
    transform: translate(-50%);
    width: 60%;
  }
  &-messages {
    bottom: 64px;
    overflow-y: auto;
  }
  &-message {
    max-width: 60%;
    padding: 10px;
    margin: 5px;
    border-radius: 10px;
    &_right {
      text-align: right;
      background-color: var(--primary-chat-color);
      margin-left: auto;
    }
    &_left {
      margin-right: auto;
      background-color: var(--secondary-chat-color);
      text-align: left;
    }
  }
  &-controller {
    bottom: 0;
    overflow: hidden;
    border-radius: 18px;
    background-color: var(--clr-white);
    &-input {
      border: 0;
      width: 100%;
      padding: 12px 42px 12px 10px;
    }
    &-button {
      position: absolute;
      top: 50%;
      right: 5px;
      transform: translateY(-50%);
      transition: background-color 0.3s;
      border: none;
      border-radius: 50%;
      width: 32px;
      height: 32px;
      background-color: #0088CC;
      cursor: pointer;
      color: var(--clr-white);
      &:hover {
        background-color: #006699;
      }
    }
  }
}
</style>
