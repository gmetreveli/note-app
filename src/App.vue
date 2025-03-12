<script setup>
import { ref } from "vue";
import Sidebar from "./components/Sidebar.vue";

const notes = ref([]);
const activeNote = ref(null);
const inputTitle = ref("");
const inputContent = ref("");
const isEditing = ref(false);

// Create Note
function createNote() {
  const id = Math.random().toString(36).substring(2, 9);

  notes.value.push({
    id,
    title: "Untitled",
    content: "",
  });

  setActiveNote(id);

  isEditing.value = true;
}

// Set Active Note
function setActiveNote(id) {
  activeNote.value = id;

  let note = notes.value.find((note) => note.id === id);

  inputTitle.value = note.title;
  inputContent.value = note.content;
  isEditing.value = true;

  setTimeout(() => {
    document.querySelector('input[type="text"]').focus();
  }, 0);
}

// Save Note & Hide Inputs
function saveNote() {
  if (!activeNote.value) return;

  let noteIndex = notes.value.findIndex((note) => note.id === activeNote.value);
  if (noteIndex !== -1) {
    notes.value[noteIndex].title = inputTitle.value;
    notes.value[noteIndex].content = inputContent.value;
  }

  isEditing.value = false; // Hide input fields after saving
}

// Delete Note
function deleteNote({ id, evt }) {
  evt.stopPropagation();
  let noteIndex = notes.value.findIndex((note) => note.id === id);
  if (noteIndex !== -1) {
    notes.value.splice(noteIndex, 1);

    // If the deleted note was active, clear the active state
    if (activeNote.value === id) {
      activeNote.value = null;
      inputTitle.value = "";
      inputContent.value = "";
      isEditing.value = false;
    }
  }
}

// Update Note
function updateNote() {
  let note = notes.value.findIndex((note) => note.id === activeNote.value);

  notes.value[note].title = inputTitle.value;
  notes.value[note].content = inputContent.value;
  isEditing.value = true;
}
</script>

<template>
  <div
    class="bg-gray-700 text-white min-h-screen flex items-stretch justify-start"
  >
    <!-- Siebar -->
    <Sidebar
      :activeNote="activeNote"
      :notes="notes"
      @new-note="createNote"
      @set-active-note="setActiveNote"
      @delete-note="deleteNote"
    />

    <!-- Main Content -->
    <main class="flex-1 relative">
      <div
        v-if="activeNote && isEditing"
        class="px-4 py-8 flex flex-col h-full"
      >
        <input
          v-model="inputTitle"
          @input="updateNote"
          type="text"
          class="block w-full text-3xl pb-2 font-bold border-b-2 border-gray-500 focus:border-white outline-none transition-colors duration-200"
        />
        <textarea
          v-model="inputContent"
          @input="updateNote"
          class="block w-full h-full mt-4 text-lg outline-none flex-1"
        ></textarea>
      </div>

      <!-- Save Button (Only Visible When isEditing is True) -->
      <button
        v-if="isEditing"
        @click="saveNote"
        class="fixed bottom-6 right-6 bg-green-500 hover:bg-green-600 cursor-pointer text-white px-6 py-3 rounded-full shadow-lg text-lg"
      >
        Save
      </button>
    </main>
  </div>
</template>
