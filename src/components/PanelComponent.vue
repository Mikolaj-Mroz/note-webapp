<template>
  <v-sheet
    :class="'bg-white ' + (isMobile ? 'mobile-panel' : 'web-panel')"
    :elevation="1"
    rounded="lg"
  >
    <v-row no-gutters>
      <v-col cols="2" md="1">
        <v-icon icon="mdi-menu" class="label-btn"></v-icon>
      </v-col>
      <v-col cols="8" md="10" class="pl-1">
        <h2 class="text-body-h6 font-weight-bold" style="line-height: 40px">
          {{ tab.name }}
        </h2>
      </v-col>
      <v-col cols="2" md="1" class="text-right">
        <v-btn icon flat @click.prevent="$emit('openTabCreate')" class="label-btn">
          <v-icon icon="mdi-plus"></v-icon>
        </v-btn>
      </v-col>
    </v-row>
    <v-divider></v-divider>
    <CreateNoteComponent
      :tab="tab"
      @createNote="$emit('createNote', tab.id)"
      @updateTitle="$emit('updateTitle', $event)"
      @updateContent="$emit('updateContent', $event)"
      @updateImportance="$emit('updateImportance', $event)"
    />
    <draggable group="people" :list="tab.notes" itemKey="title" class="drag-area">
      <template #item="{ element }">
        <NoteComponent
          :element="element"
          @openContent="element.openContent = !element.openContent"
          @openDeleteDialog="element.deleteDialog = !element.deleteDialog"
          @deleteNote="$emit('deleteNote', tab.id, element.id)"
          :tab="tab"
        />
      </template>
    </draggable>
  </v-sheet>
</template>

<script setup>
import draggable from 'vuedraggable'
import { useDisplay } from 'vuetify'
import { defineProps, defineEmits } from 'vue'
import NoteComponent from '@/components/NoteComponent.vue'
import CreateNoteComponent from './CreateNoteComponent.vue'

const isMobile = useDisplay().mobile.value

// eslint-disable-next-line no-unused-vars
const props = defineProps({
  tab: {
    type: Object,
    required: true
  }
})

// eslint-disable-next-line no-unused-vars
const emits = defineEmits([
  'openTabCreate',
  'createNote',
  'deleteNote',
  'updateTitle',
  'updateContent',
  'updateImportance'
])
</script>

<style scoped>
.web-panel {
  min-height: 70vh;
  height: 100%;
}

.mobile-panel {
  min-height: 30vh;
  height: 100%;
}

.height-fill {
  height: 100%;
}

.label-btn {
  background-color: transparent;
  height: 40px !important;
  width: 40px !important;
}

.drag-area {
  min-height: 10vh;
}

.inner-shadow {
  box-shadow: inset 0 5px 5px rgba(0, 0, 0, 0.1);
}
</style>
