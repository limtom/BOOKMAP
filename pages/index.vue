<template>
  <v-container fluid class="fill-height pa-0">
    <v-row no-gutters class="">
      <v-col><Map /></v-col>
    </v-row>
  </v-container>
</template>

<script>
import Map from '~/components/Map.vue'
export default {
  components: {
    Map,
  },
  data() {
    return {}
  },
}
</script>

<style lang="scss" scoped></style>
