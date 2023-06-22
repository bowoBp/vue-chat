<template>
  <div>
    <h1>Real-Time Chat</h1>
    <div id="chatContainer">
      <div v-for="msg in messages" :key="msg" class="message">{{ msg }}</div>
    </div>
    <input
      v-model="message"
      type="text"
      placeholder="Enter message"
      @keyup.enter="sendMessage"
    />
    <button @click="sendMessage">Send</button>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from "vue";

export default {
  setup() {
    const message = ref("");
    const messages = ref([]);

    const sendMessage = () => {
      if (message.value) {
        messages.value.push("You: " + message.value);
        localStorage.setItem("chatMessage", message.value);
        message.value = "";
      }
    };

    onMounted(() => {
      window.addEventListener("storage", handleStorageEvent);
    });

    onUnmounted(() => {
      window.removeEventListener("storage", handleStorageEvent);
    });

    const handleStorageEvent = (event) => {
      if (event.key === "chatMessage") {
        messages.value.push("Other: " + event.newValue);
      }
    };

    const storedMessage = localStorage.getItem("chatMessage");
    if (storedMessage) {
      messages.value.push("Other: " + storedMessage);
      localStorage.removeItem("chatMessage");
    }

    return {
      message,
      messages,
      sendMessage,
    };
  },
};
</script>

<style>
#chatContainer {
  margin-bottom: 10px;
}
.message {
  margin-bottom: 5px;
}
</style>
