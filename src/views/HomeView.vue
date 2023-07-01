<template>
  <main>
    <button @click="openCreateNote = !openCreateNote">Create</button>
    <form v-if="openCreateNote">
      <br>
      <label>Title:</label>
      <input type="text" v-model="noteCreateTitle"><br><br>
      <label>Content:</label>
      <input type="text" v-model="noteCreateContent"><br><br>
      <button @click.prevent="createNote()">Submit</button>
    </form>
    <div v-for="note in notes" :key="note.id">
      <h2>{{ note.title }}</h2>
      <p>{{ note.content }}</p>
      <button @click.prevent="openEdit(note)">Edit</button>
      <button @click.prevent="deleteNote(note.id)">Delete</button>
      <div v-if="openEditId == note.id">
        <label>Title:</label>
        <input type="text" @input="noteEditTitle = $event.target.value" :value="note.title"><br><br>
        <label>Content:</label>
        <input type="text" v-model="noteEditContent"><br><br>
        <button @click.prevent="editNote(note.id)">Submit</button>
      </div>
    </div>
  </main>
</template>

<script setup>

import { ref } from 'vue'

const notes = ref([])

// Create note
const openCreateNote = ref(false)
const noteCreateTitle = ref('')
const noteCreateContent = ref('')

// Edit note
const openEditId = ref(0)
const noteEditTitle = ref('')
const noteEditContent = ref('')

const createNote = () => {
  if (noteCreateTitle.value && noteCreateContent.value) {
    notes.value.push({
      id: Date.now(),
      title: noteCreateTitle.value,
      content: noteCreateContent.value
    })
    openCreateNote.value = false
    noteCreateTitle.value = null
    noteCreateContent.value = null
  }
}

const openEdit = (note) => {
  openEditId.value = note.id
  noteEditTitle.value = note.title
  noteEditContent.value = note.content
}

const editNote = (id) => {
  const note = notes.value.find(note => note.id === id)
  if (noteEditTitle.value && noteEditContent.value) {
    note.title = noteEditTitle.value
    note.content = noteEditContent.value
    openEditId.value = 0
  }
}

const deleteNote = (id) => {
  notes.value = notes.value.filter(note => note.id !== id)
}

</script>