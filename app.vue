<script setup lang="ts">
import { ref } from "vue";
import courseData from "@/assets/data.json";

// Extract the Items array from the JSON structure
const treeData = ref(courseData.items.default);

const selectedResource = ref<any>(null);
const apiResponse = ref<string>("");

async function handleResourceSelect(item: any) {
  console.log("Resource clicked:", item);
  selectedResource.value = item;
  apiResponse.value = "Loading...";

  // Simulate API call
  try {
    // In a real app: const res = await fetch(\`/api/resource/\${item['item-code']}\`)
    console.log(`Calling API for item-code: ${item["item-code"]}`);

    await new Promise((resolve) => setTimeout(resolve, 500)); // Fake delay

    apiResponse.value = JSON.stringify(
      {
        message: "Success",
        code: item["item-code"],
        name: item.name,
        type: item["item-type"],
      },
      null,
      2,
    );
  } catch (err) {
    apiResponse.value = "Error fetching data";
  }
}
</script>

<template>
  <div class="app-container">
    <header>
      <h1>{{ courseData.meta.title }}</h1>
      <p class="subtitle">{{ courseData.meta.description }}</p>
    </header>

    <div class="content-wrapper">
      <aside class="sidebar">
        <div class="tree-container">
          <TreeView :items="treeData" @select-resource="handleResourceSelect" />
        </div>
      </aside>

      <main class="main-content">
        <div v-if="selectedResource" class="resource-viewer">
          <h2>Selected Resource</h2>
          <div class="info-card">
            <h3>{{ selectedResource.name }}</h3>
            <p><strong>Code:</strong> {{ selectedResource["item-code"] }}</p>
            <p><strong>Type:</strong> {{ selectedResource["item-type"] }}</p>
          </div>

          <div class="api-response">
            <h4>API Response</h4>
            <pre>{{ apiResponse }}</pre>
          </div>
        </div>
        <div v-else class="placeholder">
          <p>Select a resource from the tree to view details</p>
        </div>
      </main>
    </div>
  </div>
</template>

<style scoped>
.app-container {
  font-family:
    -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu,
    Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  height: 100vh;
  display: flex;
  flex-direction: column;
}

header {
  margin-bottom: 20px;
  border-bottom: 1px solid #eee;
  padding-bottom: 10px;
}

h1 {
  margin: 0 0 10px 0;
  color: #2c3e50;
}

.subtitle {
  color: #666;
  font-size: 0.9em;
  line-height: 1.4;
  margin: 0;
}

.content-wrapper {
  display: flex;
  gap: 20px;
  flex: 1;
  min-height: 0; /* Important for scrolling */
}

.sidebar {
  width: 350px;
  display: flex;
  flex-direction: column;
}

.tree-container {
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 8px;
  background-color: #f9f9f9;
  overflow-y: auto;
  flex: 1;
}

.main-content {
  flex: 1;
  border: 1px solid #eee;
  border-radius: 8px;
  padding: 20px;
  background: white;
  overflow-y: auto;
}

.placeholder {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #999;
  font-style: italic;
}

.info-card {
  background: #f0f7ff;
  padding: 15px;
  border-radius: 6px;
  border-left: 4px solid #1976d2;
  margin-bottom: 20px;
}

.api-response {
  background: #282c34;
  color: #abb2bf;
  padding: 15px;
  border-radius: 6px;
  overflow-x: auto;
}

pre {
  margin: 0;
  font-family: "Consolas", "Monaco", monospace;
}
</style>
