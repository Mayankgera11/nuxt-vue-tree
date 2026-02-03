<script setup lang="ts">
import { ref, computed } from "vue";

const props = defineProps<{
  item: {
    name: string;
    "item-type": string;
    "item-code"?: string;
    items?: any[];
    embedded?: any[];
    [key: string]: any;
  };
}>();

const emit = defineEmits<{
  (e: "select-resource", item: any): void;
}>();

const isOpen = ref(false);

// Determine if it's a folder or resource
const isFolder = computed(() => props.item["item-type"] === "folder");
const isResource = computed(() => props.item["item-type"] === "resource");

// Children includes both 'items' and 'embedded'
const children = computed(() => {
  const items = props.item.items || [];
  const embedded = props.item.embedded || [];
  return [...items, ...embedded];
});

const hasChildren = computed(() => children.value.length > 0);

function toggle(e?: Event) {
  if (e) e.stopPropagation();
  if (hasChildren.value) {
    isOpen.value = !isOpen.value;
  }
}

function handleRowClick() {
  if (isResource.value) {
    emit("select-resource", props.item);
  } else {
    toggle();
  }
}

function onSelectResource(item: any) {
  emit("select-resource", item);
}
</script>

<template>
  <li class="tree-item">
    <div
      @click.stop="handleRowClick"
      class="item-content"
      :class="{
        'is-folder': isFolder,
        'is-resource': isResource,
        'is-open': isOpen,
      }"
    >
      <!-- Expand/Collapse Toggle -->
      <span v-if="hasChildren" class="icon toggle-icon" @click.stop="toggle">
        {{ isOpen ? "▼" : "▶" }}
      </span>
      <span v-else class="icon spacer"></span>

      <!-- Type Icon -->
      <span v-if="isFolder" class="icon type-icon">📁</span>
      <span v-else class="icon type-icon">📄</span>

      <span class="label">{{ item.name }}</span>
    </div>

    <div v-if="isOpen && hasChildren" class="children-container">
      <TreeView :items="children" @select-resource="onSelectResource" />
    </div>
  </li>
</template>

<style scoped>
.tree-item {
  margin: 4px 0;
}

.item-content {
  display: flex;
  align-items: center;
  cursor: pointer;
  padding: 6px 8px;
  border-radius: 4px;
  transition: background-color 0.2s;
}

.item-content:hover {
  background-color: #f0f0f0;
}

.item-content.is-resource .label {
  color: #2c3e50;
}

.item-content.is-resource:hover {
  background-color: #e3f2fd;
  color: #1976d2;
}

.icon {
  display: inline-flex;
  justify-content: center;
  align-items: center;
}

.toggle-icon {
  width: 20px;
  margin-right: 4px;
  font-size: 0.8em;
  color: #666;
  cursor: pointer;
}

.toggle-icon:hover {
  color: #333;
}

.type-icon {
  margin-right: 8px;
  font-size: 1.1em;
}

.spacer {
  width: 20px;
  margin-right: 4px;
}

.label {
  font-weight: 500;
}

.children-container {
  overflow: hidden;
  margin-left: 10px; /* Indent children slightly more */
}
</style>
