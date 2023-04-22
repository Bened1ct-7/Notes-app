<script setup lang="ts">
interface Note {
  id: number;
  title: string;
  description: string;
  date: string;
}

defineProps<{
  note: Note;
}>();

const emit = defineEmits<{
  (e: "change", value: number): void;
}>();

const deleteNote = (id: number): void => {
  emit("change", id);
}
</script>

<template>
  <div v-if="note" class="note">
    <header>
      <h2>{{ note.title }}</h2>
    </header>
    <div class="body">
      <p>
        {{ note.description }}
      </p>
    </div>
    <footer class="d-flex-between">
      <span>{{ note.date }}</span>
      <div @click="deleteNote(note.id)">
        <i class="bi bi-trash"></i>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.note {
  background: #fff;
  padding: 0.8rem;
  border-radius: 4px;
  box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);
  transition: transform 0.3s ease;
}

.note:hover {
  transform: scale(1.03);
}

header {
  margin-bottom: 0.5rem;
}

h2 {
  font-size: 1.05rem;
  font-weight: 600;
  color: #111;
  opacity: 0.75;
}

.body {
  margin-bottom: 0.6rem;
}

.body p {
  font-size: 0.9rem;
  color: #555;
}

footer {
  color: #999;
}

footer span {
  font-size: 0.85rem;
}

footer div {
  background: #eee;
  padding: 0.1rem 0.4rem;
  border-radius: 2px;
  transition: all 0.3s ease;
}

footer div i {
  font-size: 1rem;
  transition: all 0.3s ease;
  cursor: pointer;
}

footer div:hover i {
  transform: scale(0.97);
}

@media only screen and (min-width: 768px) {
  .note {
    padding: 1.5rem;
  }

  .body p {
    font-size: 0.85rem;
  }
}

@media only screen and (max-width: 768px) {
  .note {
    width: 85%;
    max-width: 300px;
    margin: auto;
  }
}
</style>
