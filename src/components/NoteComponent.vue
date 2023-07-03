<template>
  <div>
    <v-sheet
      @click.prevent="$emit('openContent')"
      :color="element.color"
      class="ma-1 pa-2 drag-object"
      :rounded="element.openContent && element.content ? 't-lg' : 'lg'"
      :elevation="2"
    >
      <v-row>
        <v-col>
          <h3 class="text-body-1 font-weight-bold">
            {{ element.title }}
          </h3>
        </v-col>
        <v-col cols="3" class="text-right">
          <v-dialog width="auto">
            <template v-slot:activator="{ props }">
              <v-btn
                @click.prevent="$emit('openDeleteDialog')"
                @keyup.enter="$emit('deleteNote', tab.id, element.id)"
                class="note-btn"
                flat
                icon
                v-bind="props"
              >
                <v-icon icon="mdi-close" color="white" size="x-small"></v-icon>
              </v-btn>
            </template>
            <v-card class="text-center">
              <v-card-text>
                <p class="text-body-1">Are you sure you want to delete this note?</p>
              </v-card-text>
              <v-card-actions>
                <v-btn @click.prevent="$emit('deleteNote', tab.id, element.id)" block>
                  Delete
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-col>
      </v-row>
    </v-sheet>
    <v-expand-transition class="mt-n1">
      <div v-if="element.openContent && element.content">
        <!--- helps make animation smoother -->
        <v-sheet
          class="px-2 pt-2 mx-1 inner-shadow"
          rounded="b-lg"
          :color="element.color"
          style="min-height: 64px"
        >
          <p class="text-body-2 text-justify">{{ element.content }}</p>
        </v-sheet>
      </div>
    </v-expand-transition>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

// eslint-disable-next-line no-unused-vars
const props = defineProps({
  element: {
    type: Object,
    required: true
  },
  tab: {
    type: Object,
    required: true
  }
})

// eslint-disable-next-line no-unused-vars
const emit = defineEmits(['deleteNote', 'openContent', 'openDeleteDialog'])
</script>

<style scoped>
.note-btn {
  background-color: transparent;
  height: 24px !important;
  width: 24px !important;
}

.drag-object {
  cursor: pointer;
}
</style>
