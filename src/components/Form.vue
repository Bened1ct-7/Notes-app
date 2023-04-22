<script setup lang="ts">
import { ref } from "vue";

interface Note {
  id: number;
  title: string;
  description: string;
  date: string;
}

const emit = defineEmits<{
  (e: "update"): void;
  (e: "change"): void;
}>();

const hideForm = (): void => {
  emit("update");
};

const noteTitle = ref<string>("");
const noteDesc = ref<string>("");

const getNotes = (): Note[] => {
  let notes: Note[] = [];
  if (localStorage.getItem("notes")) {
    notes = JSON.parse(localStorage.getItem("notes"));
  }
  return notes;
};

const months: string[] = [
  "January",
  "February",
  "March",
  "April",
  "May",
  "June",
  "July",
  "August",
  "September",
  "October",
  "November",
  "December",
];

let notes: Note[] | null = getNotes();

const addNote = (): void => {
  if (noteTitle.value === "" || noteDesc.value === "") {
    alert("All fields are required");
    return;
  }

  notes = getNotes();
  let id: number = Math.floor(Math.random() * 10000000000);

  const date: Date = new Date();
  const year: string | number = date.getFullYear();
  const month: string = months[date.getMonth()];
  const day: string | number = date.getDate();
  let hours: string | number =
    date.getHours() > 12 ? date.getHours() - 12 : date.getHours();
  let minutes: string | number = date.getMinutes();
  let amPm: string = date.getHours() >= 12 ? "PM" : "AM";
  let formattedMinutes: string | number =
    minutes < 10 ? "0" + minutes : minutes;

  const fullDate: string = `${month} ${day}, ${year} ${hours}:${formattedMinutes} ${amPm}`;

  const note: Note = {
    id: id,
    title: noteTitle.value,
    description: noteDesc.value,
    date: fullDate,
  };

  notes.push(note);
  localStorage.setItem("notes", JSON.stringify(notes));
  emit("change");
};
</script>

<template>
  <section class="d-flex-center">
    <div class="container">
      <header class="d-flex-between">
        <h2>ADD NEW NOTE</h2>
        <i @click="hideForm" class="bi bi-x"></i>
      </header>
      <form @submit.prevent="addNote" action="#">
        <div>
          <label for="title">Title</label>
          <input
            type="text"
            name="title"
            placeholder="title"
            v-model="noteTitle"
          />
        </div>
        <div>
          <label for="description">Description</label>
          <textarea
            name="description"
            rows="8"
            cols="40"
            v-model="noteDesc"
          ></textarea>
        </div>
        <div>
          <button type="submit">Add note</button>
        </div>
      </form>
    </div>
  </section>
</template>

<style scoped>
section {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  min-height: 100vh;
  background: rgba(0, 0, 0, 0.5);
  z-index: 9;
}

.container {
  width: 83%;
  max-width: 400px;
  margin: auto;
  border-radius: 8px;
  padding: 1rem;
  background: #fff;
}

header {
  padding-bottom: 1rem;
  border-bottom: 1px solid #ccc;
  margin-bottom: 0.8rem;
}

header i {
  font-size: 2rem;
  color: crimson;
  transition: all 0.3s ease;
}

header i:hover {
  font-size: 1.95rem;
}

h2 {
  font-size: 1.13rem;
  letter-spacing: 0.8px;
  opacity: 0.75;
  color: #333;
}

div {
  margin-bottom: 0.8rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  letter-spacing: 0.6px;
  font-size: 0.95rem;
  color: #333;
}

input,
textarea {
  appearance: none;
  width: 100%;
  border-radius: 3px;
  border: none;
  background: #eee;
  font-size: 0.95rem;
}

input:focus,
textarea:focus {
  outline: 1px solid var(--primary);
}

input {
  height: 45px;
  padding: 0 1rem;
}

textarea {
  padding: 0.5rem;
}

button {
  border: none;
  background: var(--primary);
  color: #fff;
  border-radius: 2px;
  padding: 0.5rem 1rem;
  transition: all 0.3s ease;
}

button:hover {
  transform: scale(0.97);
}
</style>
