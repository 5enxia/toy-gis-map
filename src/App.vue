<script setup lang="ts">
import { computed, ref } from "vue";
import Tiles from "./components/Tiles.vue";
import type { Idx } from "./components/Tiles.vue";

const MAX_ZOOM = 20;
const zoomLevel = ref(1);

const length = computed(() => Math.pow(2, zoomLevel.value));

// tile indexes
const idx = ref<Idx>([0, 0]);
const idxs = computed(() => {
  const tmpIdxs = [
    [idx.value[0], idx.value[1]],
    [idx.value[0] + 1, idx.value[1]],
    [idx.value[0], idx.value[1] + 1],
    [idx.value[0] + 1, idx.value[1] + 1],
  ];
  return tmpIdxs.map<Idx>((idx) => {
    return [idx[0] % length.value, idx[1] % length.value];
  });
});

window.addEventListener("keypress", (e) => {
  // zoom in
  if (e.key === "=") {
    zoomLevel.value = zoomLevel.value + 1;
    if (zoomLevel.value > MAX_ZOOM) {
      zoomLevel.value = MAX_ZOOM;
      return;
    }
    idx.value[0] = idx.value[0] * 2;
    idx.value[1] = idx.value[1] * 2;
  }
  // zoom out
  if (e.key === "-") {
    zoomLevel.value = zoomLevel.value - 1;
    if (zoomLevel.value < 1) {
      zoomLevel.value = 1;
      return;
    }
    idx.value[0] = Math.floor(idx.value[0] / 2);
    idx.value[1] = Math.floor(idx.value[1] / 2);
  }
  // up
  if (e.key === "w") {
    idx.value[1] = idx.value[1] - 1;
    if (idx.value[1] < 0) {
      idx.value[1] = length.value - 1;
    }
  }
  // left
  if (e.key === "a") {
    idx.value[0] = idx.value[0] - 1;
    if (idx.value[0] < 0) {
      idx.value[0] = length.value - 1;
    }
  }
  // down
  if (e.key === "s") {
    idx.value[1] = idx.value[1] + 1;
    if (idx.value[1] >= length.value) {
      idx.value[1] = 0;
    }
  }
  // right
  if (e.key === "d") {
    idx.value[0] = idx.value[0] + 1;
    if (idx.value[0] >= length.value) {
      idx.value[0] = 0;
    }
  }
});

const keyConfigs = {
  "=": "zoom in",
  "-": "zoom out",
  w: "up",
  a: "left",
  s: "down",
  d: "right",
};
</script>

<template>
  <h1>Toy GIS Map</h1>
  <!-- keyConfigs -->
  <table>
    <thead>
      <tr>
        <th>key</th>
        <th>action</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(action, key) in keyConfigs" :key="key">
        <td>{{ key }}</td>
        <td>{{ action }}</td>
      </tr>
    </tbody>
  </table>

  <!-- status -->
  <div>zoom level: {{ zoomLevel }}, focus tile: {{ idx }}</div>
  <Tiles :zoom-level="zoomLevel" :idxs="idxs"></Tiles>
</template>

<style scoped></style>
