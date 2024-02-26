<script setup lang="ts">
import { computed } from "vue";

export interface TileProps {
  z?: number;
  x: number;
  y: number;
}

const props = withDefaults(defineProps<TileProps>(), {
  z: 1,
  x: 0,
  y: 0,
});

const TEMPLATE = "http://tile.openstreetmap.org/{z}/{x}/{y}.png";
const src = computed(() => {
  return TEMPLATE.replace("{z}", props.z.toString())
    .replace("{x}", props.x.toString())
    .replace("{y}", props.y.toString());
});
</script>

<template>
  <img class="logo" alt="Vue logo" :src="src" />
</template>
