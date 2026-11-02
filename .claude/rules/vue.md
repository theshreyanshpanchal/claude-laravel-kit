---
paths:
  - "resources/js/**/*.vue"
  - "resources/js/**/*.js"
  - "resources/js/**/*.ts"
---

# Vue.js Frontend

- Use Vue 3 with the Composition API and `<script setup>` (TypeScript if the project uses it).
- Component names in PascalCase, multi-word (`OrderList.vue`, not `List.vue`). One component per file.
- Folder structure: `components/` (reusable UI), `pages/` or `views/` (route-level), `composables/` (`useOrders.js`), `services/` or `api/` (HTTP calls), `stores/` (Pinia, only if shared state is needed).
- Define `props` with types and defaults (`defineProps`), and declare `emits` (`defineEmits`). Never mutate props.
- Keep API calls out of components: use a single configured axios instance (base URL, auth header, error interceptor) in a service/composable.
- Handle loading, empty and error states for every async call.
- Use `computed` for derived data, `watch` only when side effects are needed.
- Use `v-for` with a stable `:key` (the record id). Never combine `v-if` and `v-for` on the same element.
- Keep components small; split when a component grows beyond ~200 lines.
- No `console.log` left behind. No unused components, imports or variables.
- If using Inertia, follow Inertia conventions (page components, `useForm`, shared props) and keep validation on the backend.
