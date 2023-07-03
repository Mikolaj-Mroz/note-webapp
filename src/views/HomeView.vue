<template>
  <v-row no-gutters class="ma-0 ma-md-2 mt-0 mt-md-12">
    <v-col cols="12" md="10" offset-md="1">
      <v-sheet class="bg-grey-lighten-4" rounded="lg" :elevation="24">
        <TheHeader />

        <v-row no-gutters class="px-1 py-2">
          <v-col class="px-1 py-2 py-md-0" cols="12" md="4" v-for="tab in tabs" :key="tab.id">
            <PanelComponent
              :tab="tab"
              @updateTitle="tab.form.title = $event"
              @updateContent="tab.form.content = $event"
              @updateImportance="tab.form.importance = $event"
              @openTabCreate="tab.create = !tab.create"
              @createNote="createNote"
              @deleteNote="deleteNote"
            />
          </v-col>
        </v-row>
      </v-sheet>
    </v-col>
  </v-row>
</template>

<script setup>
import { ref } from 'vue'
import PanelComponent from '@/components/PanelComponent.vue'
import TheHeader from '@/components/TheHeader.vue'

const tabs = ref([
  {
    id: 0,
    name: 'Ideas',
    notes: [],
    create: false,
    form: { title: '', content: '', importance: 'red', deleteDialog: false }
  },
  {
    id: 1,
    name: 'In progress',
    notes: [],
    create: false,
    form: { title: '', content: '', importance: 'red', deleteDialog: false }
  },
  {
    id: 2,
    name: 'Done',
    notes: [],
    create: false,
    form: { title: '', content: '', importance: 'red', deleteDialog: false }
  }
])

const createNote = (id) => {
  if (tabs.value[id].form.title) {
    tabs.value[id].notes.push({
      id: Date.now(),
      title: tabs.value[id].form.title,
      content: tabs.value[id].form.content,
      color: tabs.value[id].form.importance,
      openContent: false
    })
    tabs.value[id].form.title = ''
    tabs.value[id].form.content = ''
    tabs.value[id].form.importance = 'red'
    tabs.value[id].create = false
  }
}

const deleteNote = (tabId, noteId) => {
  tabs.value[tabId].notes = tabs.value[tabId].notes.filter((note) => note.id !== noteId)
}
</script>
