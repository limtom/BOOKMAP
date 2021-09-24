<template>
  <v-card class="mx-auto" elevation="10" tile max-width="350" height="450">
    <v-avatar class="ma-8 mb-0">
      <img :src="require('~/static/v.png')" alt="Logo" />
    </v-avatar>
    <p class="text-center text-h5 font-weight-thin blue--text text-capitalize">
      {{ action }}
    </p>
    <v-divider class="mb-7 ml-4 mr-5"></v-divider>
    <v-card-text class="pt-0 pb-0 pl-7 pr-7">
      <v-text-field
        v-model="email"
        dense
        type="text"
        error-count=""
        placeholder=""
        label="Email"
        append-icon=""
        outlined
        color
      ></v-text-field>

      <v-text-field
        v-model="password"
        dense
        type="password"
        error-count=""
        placeholder=""
        label="Password"
        append-icon="mdi-eye"
        outlined
        color
      ></v-text-field>
      <v-text-field
        v-if="isSignup"
        v-model="repeatPassword"
        dense
        type="password"
        error-count=""
        placeholder=""
        label="Repeat Password"
        append-icon="mdi-eye"
        outlined
        color
      ></v-text-field>
    </v-card-text>
    <v-card-actions class="d-flex flex-column">
      <v-btn color="primary" class="mx-auto" outlined width="100">
        {{ action }}
      </v-btn>
      <p class="text-caption mt-4">
        {{
          action == 'Sign-up'
            ? 'Already have an account'
            : 'Register for an account'
        }}
        <nuxt-link :to="route" class="text-decoration-none">{{
          action == 'Sign-up' ? 'Sign-in' : 'Sign-up'
        }}</nuxt-link>
      </p>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    action: {
      type: String,
      default: 'Login',
    },

    isSignup: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      email: '',
      password: '',
      repeatPassword: '',
    }
  },
  computed: {
    route() {
      return this.action === 'Sign-up' ? '/sign-in' : '/sign-up'
    },
  },
  methods: {},
}
</script>

<style lang="scss" scoped></style>
