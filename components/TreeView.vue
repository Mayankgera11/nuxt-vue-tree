<script setup lang="ts">
defineProps<{
  items: any[];
}>();

const emit = defineEmits<{
  (e: "select-resource", item: any): void;
}>();

function onSelectResource(item: any) {
  emit("select-resource", item);
}
</script>

<template>
  <ul class="tree-view">
    <TreeItem
      v-for="(item, index) in items"
      :key="index"
      :item="item"
      @select-resource="onSelectResource"
    />
  </ul>
</template>

<style scoped>
.tree-view {
  list-style-type: none;
  padding-left: 20px;
  margin: 0;
}
</style>
