<template>
  <div v-if="row_clicked != 0 && col_clicked != 0">
    <button
      style="background-color: #ff0000"
      @click="set_color('#ff0000', row_clicked, col_clicked)"
    ></button>
    <button
      style="background-color: #00ff00"
      @click="set_color('#00ff00', row_clicked, col_clicked)"
    ></button>
    <button
      style="background-color: #0000ff"
      @click="set_color('#0000ff', row_clicked, col_clicked)"
    ></button>
  </div>

  <p>Col:{{ col_clicked }}, Row:{{ row_clicked }} clicked, {{ status }}</p>
  <ColorCanvas :colors="colors" :paint="paint" @select="select" />
</template>

<script setup>
import ColorCanvas from "./components/ColorCanvas.vue";
import { ref } from "vue";

const row_clicked = ref(0);
const col_clicked = ref(0);
// available or unavailable
const status = ref("");
// from left to right
const colors = ref([
  "#ff0000",
  "#00ff00",
  "#0000ff",
  "#ffff00",
  "#00ffff",
  "#00ffff",
]);

// if you want to paint one cell, please change the paint value
const paint = ref({});

const select = (payload) => {
  console.log(payload);
  col_clicked.value = payload.col_index;
  row_clicked.value = payload.row_index;
  status.value = payload.status;
};
const set_color = (color, row_index, col_index) => {
  console.log(row_clicked, col_clicked);
  paint.value = { color, row_index, col_index };
};
</script>

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
