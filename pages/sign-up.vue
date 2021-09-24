<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div class="text-center">
        <Regform action="Sign-up"> </Regform>
      </div>
    </v-col>
  </v-row>
</template>

<script>
import Regform from '~/components/Form.vue'

export default {
  components: {
    Regform,
  },
  layout: 'main',
  methods: {
    update(e) {
      this.$emit('updateInput', e.target.value)
    },
  },
}
</script>
