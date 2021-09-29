<template>
  <v-app>
    <v-app-bar fixed app flat>
      <v-toolbar-title v-text="title" />
    </v-app-bar>
    <v-main>
      <v-container fluid class="fill-height pa-0 justify-center">
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      title: 'BOOKMAP',
    }
  },
}
</script>
