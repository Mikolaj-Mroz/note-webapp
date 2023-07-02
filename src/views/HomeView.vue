<template>
  <v-row no-gutters class="ma-2 mt-12">
    <v-col cols="12" md="10" offset-md="1">
      <v-sheet class="bg-grey-lighten-4" rounded="lg" :elevation="24">
        <v-sheet class="bg-black" rounded="t-lg">
          <v-row no-gutters>
            <v-col cols="8" class="px-4 py-2">
              <h1 class="text-h4 font-weight-bold">KANBAN BOARD ONLINE</h1>
            </v-col>
            <v-spacer></v-spacer>
            <v-col class="py-1 px-1 text-right">
              <v-btn
                href="https://github.com/Mikolaj-Mroz"
                color="transparent"
                flat
                icon="mdi-github"
              ></v-btn>
            </v-col>
          </v-row>
        </v-sheet>
        <v-row no-gutters class="px-1 py-2">
          <v-col class="px-1" cols="4" v-for="tab in tabs" :key="tab.id">
            <v-sheet class="bg-white" :elevation="1" rounded="lg" style="min-height: 70vh; height:100%">
              <v-row no-gutters>
                <v-col cols="1">
                  <v-icon icon="mdi-menu" class="label-btn"></v-icon>
                </v-col>
                <v-col cols="9" class="pl-1">
                  <h2 class="text-body-h6 font-weight-bold" style="line-height: 40px">
                    {{ tab.name }}
                  </h2>
                </v-col>
                <v-col cols="2" class="text-right">
                  <v-btn icon flat @click.prevent="tab.create = !tab.create" class="label-btn">
                    <v-icon icon="mdi-plus"></v-icon>
                  </v-btn>
                </v-col>
              </v-row>
              <v-divider></v-divider>
              <v-expand-transition>
                <div v-if="tab.create">
                  <!--- helps make animation smoother -->
                  <v-form class="pa-2 inner-shadow"
                    @submit.prevent="createNote(tab.id)"  
                  >
                    <v-text-field v-model="tab.form.title" label="Title" required></v-text-field>
                    <v-textarea v-model="tab.form.content" label="Content" required></v-textarea>
                    <v-radio-group inline v-model="tab.form.importance">
                      <v-radio label="Important" value="red"></v-radio>
                      <v-radio label="Normal" value="orange-darken-2"></v-radio>
                      <v-radio label="Not important" value="green"></v-radio>
                      <v-radio label="Other" value="blue"></v-radio>
                    </v-radio-group>
                    <v-btn variant="outlined" block @click="createNote(tab.id)">Submit</v-btn>
                  </v-form>
                </div>
              </v-expand-transition>
              <draggable group="people" :list="tab.notes" itemKey="title" class="drag-area">
                <template #item="{ element }">
                  <div>
                    <v-sheet
                      @click.prevent="element.openContent = !element.openContent"
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
                          <v-dialog v-model="element.deleteDialog" width="auto">
                            <template v-slot:activator="{ props }">
                              <v-btn
                                @click.prevent="element.deleteDialog = !element.deleteDialog"
                                @keyup.enter="deleteNote(tab.id, element.id)"
                                class="note-btn"
                                flat
                                icon
                                v-bind="props"
                              >
                                <v-icon icon="mdi-close" color="white" size="x-small"></v-icon>
                              </v-btn>
                            </template>
                            <v-card style="width: 30vw" class="text-center">
                              <v-card-text>
                                <p>Are you sure you want to delete this note?</p>
                              </v-card-text>
                              <v-card-actions>
                                <v-btn @click.prevent="deleteNote(tab.id, element.id)" block>
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
              </draggable>
            </v-sheet>
          </v-col>
        </v-row>
      </v-sheet>
    </v-col>
  </v-row>
</template>

<script setup>
import { ref } from 'vue'
import draggable from 'vuedraggable'

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

<style scoped>
.rotate {
  rotate: 180deg;
  transition: rotate 1s;
}

.height-fill {
  height: 100%;
}

.label-btn {
  background-color: transparent;
  height: 40px !important;
  width: 40px !important;
}

.note-btn {
  background-color: transparent;
  height: 24px !important;
  width: 24px !important;
}

.drag-area {
  min-height: 10vh;
}

.drag-object {
  cursor: pointer;
}

.inner-shadow {
  box-shadow: inset 0 5px 5px rgba(0, 0, 0, 0.1);
}
</style>
