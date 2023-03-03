<script setup>
import ColorCanvas from "./components/ColorCanvas.vue";
import { ref } from "vue";
const colors = ref([
  "#ff0000",
  "#00ff00",
  "#0000ff",
  "#ffff00",
  "#00ffff",
  "#00ffff",
]);
const test = (payload) => {
  console.log(payload);
  colors.value = ["#ff0000", "#00ff00"];
};
</script>

<template>
  <!-- <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div> -->
  <ColorCanvas :colors="colors" @paint="test" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
