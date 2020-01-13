<script>
import { ipcRenderer } from 'electron';

export default {
  name: 'WindowHeader',
  data() {
    return {
      ipcRenderer,
    };
  },
};
</script>

<template>
  <v-system-bar
    fixed
    class="top"
    color="blue darken-4"
    height="30"
    flat
  >
    <v-icon class="text_white">mdi-database</v-icon>
    <span class=" text_white pl-2">My BaseDate</span>
    <div class="flex-grow-1"></div>
    <v-icon small class="icon pr-1 text_white" @click="ipcRenderer.send('minimize')">mdi-window-minimize</v-icon>
    <v-icon small class="icon pr-1 text_white" @click="ipcRenderer.send('maximize')">mdi-window-maximize</v-icon>
    <v-icon small class="icon text_white" @click="ipcRenderer.send('close')">mdi-window-close</v-icon>
  </v-system-bar>
</template>

<style scoped>
  .text_white {
    color: white !important;
  }
  .top {
    -webkit-app-region: drag;
  }
  .icon {
    -webkit-app-region: no-drag;
  }
</style>
