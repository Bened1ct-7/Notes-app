<script setup lang="ts">
import { ref } from "vue";

const emit = defineEmits<{
  (e: "update"): void;
  (e: "search", value: string): void;
}>();

const showBar = ref<boolean>(false);
const title = ref<string>("");

const showForm = (): void => {
  emit("update");
};

const searchNote = async (): void => {
  await emit("search", title.value);
}
</script>

<template>
  <header class="d-flex-center">
    <div class="container d-flex-between">
      <h1>Notes</h1>
      <div class="icons">
        <i @click="showBar = true" class="bi bi-search search"></i>
        <i @click="showForm" class="bi bi-pen add"></i>
      </div>
      <div v-if="showBar === true" class="search-bar">
        <input
          type="text"
          placeholder="Search by title"
          v-model="title"
          @input="searchNote"
        />
        <button @click="showBar = false" type="button">
          <i class="bi bi-x"></i>
        </button>
      </div>
    </div>
  </header>
</template>

<style scoped>
header {
  min-height: 60px;
  background: var(--primary);
  color: #fff;
}

.container {
  position: relative;
}

.search-bar {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
  width: 100%;
  max-width: 100%;
  height: 45px;
  background: red;
  border-radius: 3px;
  display: flex;
  align-items: center;
  padding: 0.3rem;
  background: #fff;
  gap: 2%;
  overflow: hidden;
}

input {
  appearance: none;
  display: block;
  width: 85%;
  height: 100%;
  border-radius: inherit;
  border: none;
  padding: 0 1rem;
  transition: all 0.1s ease;
  background: #eee;
  font-size: 0.92rem;
}

input:focus {
  outline: 1px solid var(--primary);
}

button {
  width: 12%;
  height: 100%;
  border-radius: inherit;
  background: var(--primary);
  display: flex;
  align-items: center;
  justify-content: center;
  border: none;
  transition: all 0.3s ease;
}

button i {
  font-size: 1.5rem;
}

button:hover {
  transform: scale(0.97);
}

h1 {
  font-size: 1.5rem;
  letter-spacing: 1px;
  font-weight: 500;
}

i {
  color: #fff;
  font-size: 1.3rem;
  margin: 0 0.8rem;
}

i.add {
  margin-right: 0;
}
</style>
