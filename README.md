# Nuxt Tree View Courseware Demo

This project demonstrates a recursive Tree View component built with Nuxt 3 and Vue 3. It visualizes a hierarchical course curriculum structure (Folders, Units, Lessons, Resources) from a complex JSON dataset and simulates API interactions.

## Features

- **Recursive Tree Rendering**: Efficiently renders deeply nested structures using a self-referential `TreeView` component.
- **Dynamic Content Handling**:
  - **Folders**: Expandable/collapsible containers.
  - **Resources**: Clickable items that trigger actions.
  - **Embedded Items**: Handles nested arrays (`items` and `embedded`) seamlessly.
- **API Simulation**: Simulates an asynchronous API fetch when a resource is selected, displaying its details and `item-code`.
- **Responsive Layout**: Sidebar navigation with a main content area.

## Project Structure

- **`app.vue`**: The main entry point. Fetches data from `assets/data.json`, manages the selected state, and handles the "mock" API calls.
- **`components/TreeView.vue`**: The container component that renders a list of `TreeItem`s.
- **`components/TreeItem.vue`**: The core component for each node. It handles:
  - Recursive rendering logic.
  - Expansion/Collapse state.
  - Event emission for resource selection.
- **`assets/data.json`**: Contains the complex hierarchical dataset used to populate the tree.

## Setup & Run

1. **Install Dependencies**:

   ```bash
   npm install
   ```

2. **Start Development Server**:
   ```bash
   npm run dev
   ```
   Open [http://localhost:3000](http://localhost:3000) to view the application.

## Usage

1. **Navigate the Tree**: Click on the arrows or folder icons to expand/collapse sections.
2. **View Embedded Items**: Resources like "Othello's Renaissance" contain embedded items that can be expanded further.
3. **Select a Resource**: Click on any document/resource name (e.g., "Quotable Connections") to "fetch" its data. The API response simulation will appear in the main content area.

## Technology Stack

- **Framework**: [Nuxt 3](https://nuxt.com/)
- **UI Engine**: [Vue 3](https://vuejs.org/) (Composition API, `<script setup>`)
- **Language**: TypeScript
