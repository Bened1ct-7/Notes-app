<script setup lang="ts">
import { ref, onMounted } from "vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import Form from "./components/Form.vue";
import NoteSection from "./components/NoteSection.vue";

interface Note {
  id: number;
  title: string;
  description: string;
  date: string;
}

const showForm = ref<boolean>(false);

const getNotes = (): Note[] => {
  let notes: Note[] = [];
  if (localStorage.getItem("notes")) {
    notes = JSON.parse(localStorage.getItem("notes"));
  }
  return notes;
};

let notes = ref<Note[]>([]);

const toggleForm = (): void => {
  showForm.value = !showForm.value;
};

const refresh = (): void => {
  showForm.value = false;
  notes.value = getNotes();
};

const deleteNote = (id: number): void => {
  notes.value = getNotes();
  notes.value = notes.value.filter((note) => note.id !== id);
  localStorage.setItem("notes", JSON.stringify(notes.value));
};

const searchNote = (term: string): void => {
  const allNotes: Note[] = getNotes();
  let newNotes: Note[] = [];
  allNotes.forEach(note => {
    if (note.title.toLowerCase().indexOf(term.toLowerCase()) !== -1) {
      newNotes.push(note);
    }
  })
  notes.value = newNotes;
}

onMounted(() => (notes.value = getNotes()));
</script>

<template>
  <Header @update="toggleForm" @search="searchNote" />
  <NoteSection :notes="notes" @update="deleteNote" />
  <Footer />
  <Form v-if="showForm" @update="toggleForm" @change="refresh" />
</template>

<style scoped></style>
