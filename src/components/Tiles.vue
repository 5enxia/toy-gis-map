<script setup lang="ts">
import Tile from "../components/Tile.vue";

export type Idx = [number, number];
interface TilesProps {
  zoomLevel: number;
  idxs: Idx[];
}

const props = withDefaults(defineProps<TilesProps>(), {
  zoomLevel: 1,
  idxs: () => [
    [0, 0],
    [1, 0],
    [0, 1],
    [1, 1],
  ],
});
</script>

<template>
  <div class="tiles" >
    <template v-for="(idx, i) in idxs">
      <Tile class="tile" :class="{'focus': i == 0 }" :z="props.zoomLevel" :x="idx[0]" :y="idx[1]" />
    </template>
  </div>
</template>

<style scoped>
.tiles {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4px;
}

.tile {
  border: 4px solid grey;
}

.focus {
    border-color: red;
}
</style>
