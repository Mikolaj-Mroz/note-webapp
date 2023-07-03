<template>
  <v-sheet class="bg-black" :rounded="isMobile ? '0' : 't-lg'">
    <v-row no-gutters>
      <v-col cols="8" class="px-2 px-md-4 py-4 py-md-2">
        <h1 class="text-body-1 text-md-h4 font-weight-bold">KANBAN BOARD ONLINE</h1>
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
</template>

<script setup>
import { useDisplay } from 'vuetify'

const isMobile = useDisplay().mobile.value
</script>
