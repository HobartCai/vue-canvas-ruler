<script setup lang="ts">
import { ref } from "vue";
import VueCanvasRuler from "./components/vue-canvas-ruler.vue";

const pixelPerUnit = ref(1);
const scale = ref(1);
const offset = ref(0);
const sparsity = ref(100);
const part = ref(10);
</script>

<template>
  <vue-canvas-ruler
    :pixelPerUnit="pixelPerUnit"
    :scale="scale"
    :offset="offset"
    :sparsity="sparsity"
    :part="part"
  />
  <div class="grid-container">
    <div><span>Pixels Per Unit :</span></div>
    <div><input v-model="pixelPerUnit" type="number" min="0" /></div>
    <div><span>Scale :</span></div>
    <div><input v-model="scale" type="number" step="0.1" min="0.1" /></div>
    <div><span>Offset :</span></div>
    <div><input v-model="offset" type="number" /></div>
    <div><span>Sparsity :</span></div>
    <div><input v-model="sparsity" type="number" min="0" /></div>
    <div><span>Part :</span></div>
    <div><input v-model="part" type="number" min="0" /></div>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.grid-container {
  display: grid;
  margin-top: 20px;
  grid-template-columns: 120px 240px;
  grid-template-rows: repeat(1, 1.6em);
  grid-auto-flow: row;
}
</style>
