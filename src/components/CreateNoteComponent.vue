<template>
  <v-expand-transition>
    <div v-if="tab.create">
      <!--- helps make animation smoother -->
      <v-form class="pa-2 inner-shadow" @submit.prevent="$emit('createNote', tab.id)">
        <v-text-field v-model="formTitleModel" label="Title" required></v-text-field>
        <v-textarea v-model="formContentModel" label="Content" required></v-textarea>
        <v-radio-group inline v-model="formImportanceModel">
          <v-radio label="Important" value="red"></v-radio>
          <v-radio label="Normal" value="orange-darken-2"></v-radio>
          <v-radio label="Not important" value="green"></v-radio>
          <v-radio label="Other" value="blue"></v-radio>
        </v-radio-group>
        <v-btn variant="outlined" block @click="$emit('createNote', tab.id)">Submit</v-btn>
      </v-form>
    </div>
  </v-expand-transition>
</template>

<script setup>
import { defineProps, defineEmits, computed } from 'vue'

// eslint-disable-next-line no-unused-vars
const props = defineProps({
  tab: {
    type: Object,
    required: true
  }
})

// eslint-disable-next-line no-unused-vars
const emit = defineEmits(['createNote', 'updateTitle', 'updateContent', 'updateImportance'])

const formTitleModel = computed({
  // getter
  get() {
    return props.tab.form.title
  },
  // setter
  set(NewValue) {
    emit('updateTitle', NewValue)
  }
})

const formContentModel = computed({
  // getter
  get() {
    return props.tab.form.content
  },
  // setter
  set(NewValue) {
    emit('updateContent', NewValue)
  }
})

const formImportanceModel = computed({
  // getter
  get() {
    return props.tab.form.importance
  },
  // setter
  set(NewValue) {
    emit('updateImportance', NewValue)
  }
})
</script>
