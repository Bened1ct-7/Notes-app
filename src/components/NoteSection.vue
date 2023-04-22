<script setup lang="ts">
import { ref, watch } from "vue";
import NoteItem from "./NoteItem.vue";

const emit = defineEmits<{
  (e: "update", value: number): void;
}>();

interface Note {
  id: number;
  title: string;
  description: string;
  date: string;
}

defineProps<{
  notes: Note[];
}>();

const change = (id: number): void => {
  emit("update", id);
};
</script>

<template>
  <section>
    <div v-if="notes.length > 0" class="container notes">
      <NoteItem
        v-for="note in notes"
        :key="note.id"
        :note="note"
        @change="change"
      />
    </div>
    <div v-else class="container">
      <div class="no-notes">
        <p>You have no notes</p>
      </div>
    </div>
  </section>
</template>

<style scoped>
section {
  margin: 2rem 0;
}

.container.notes {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-row-gap: 1.2rem;
  grid-column-gap: 1.4rem;
}

.no-notes {
  text-align: center;
}

.no-notes p {
  font-size: 0.9rem;
  color: #555;
}

@media only screen and (max-width: 768px) {
  .container.notes {
    grid-template-columns: 1fr;
  }
}
</style>
