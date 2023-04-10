<template>
  <v-app-bar flat color="primary">
    <v-app-bar-title align="center">
      <v-icon icon="mdi-format-list-bulleted" />
      Framework
    </v-app-bar-title>

    <v-col cols="auto">
      <v-btn
        min-width="164"
        rel="noopener noreferrer"
        target="_blank"
        variant="text"
      >
        Add
        <v-icon
          icon="mdi-plus"
          size="large"
          start
        />
        <template>
          <AddTask />
        </template>
      </v-btn>
    </v-col>
  </v-app-bar>
</template>

<script setup>
  import AddTask from '@/layouts/default/AddTask.vue'
</script>
