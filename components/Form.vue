<template>
  <v-form ref="form" @submit.prevent="validateForm(userInfo)">
    <v-alert v-show="!!errorInfo" outlined type="error" dismissible>
      {{ errorInfo }}
    </v-alert>
    <v-card class="mx-auto" elevation="10" tile max-width="350" height="450">
      <v-avatar class="ma-8 mb-0">
        <img :src="require('~/static/v.png')" alt="Logo" />
      </v-avatar>
      <p
        class="text-center text-h5 font-weight-thin blue--text text-capitalize"
      >
        {{ action }}
      </p>
      <v-divider class="mb-7 ml-4 mr-5"></v-divider>
      <v-card-text class="pt-0 pb-0 pl-7 pr-7">
        <v-text-field
          v-model="userInfo.email"
          dense
          type="text"
          placeholder="Input your email"
          label="Email"
          append-icon=""
          outlined
          color
          :rules="[rules.required, rules.email]"
        ></v-text-field>

        <v-text-field
          v-model="userInfo.password"
          dense
          placeholder="Input your desired password"
          label="Password"
          :type="showPassword ? 'text' : 'password'"
          :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
          outlined
          color
          :rules="[rules.required, rules.counter]"
          @click:append="showPassword = !showPassword"
        ></v-text-field>
        <v-text-field
          v-if="isSignup"
          v-model="userInfo.repeatPassword"
          dense
          :type="showPassword ? 'text' : 'password'"
          :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
          placeholder="Repeat password"
          label="Repeat Password"
          outlined
          color
          :rules="[rules.required, rules.counter, rules.repeatPassword]"
          @click:append="showPassword = !showPassword"
        ></v-text-field>
      </v-card-text>
      <v-card-actions class="d-flex flex-column">
        <v-btn
          color="primary"
          class="mx-auto"
          type="submit"
          outlined
          width="100"
        >
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
  </v-form>
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
      userInfo: {
        email: '',
        password: '',
        repeatPassword: '',
      },
      showPassword: false,
      rules: {
        required: (value) => !!value || 'Required.',
        counter: (value) => value.length > 6 || 'Min 6 characters',
        email: (value) => {
          const pattern =
            /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          return pattern.test(value) || 'Invalid e-mail.'
        },
        repeatPassword: (value) =>
          value === this.userInfo.password || 'Password mis-match',
      },
      errorInfo: '',
    }
  },
  computed: {
    route() {
      return this.action === 'Sign-up' ? '/sign-in' : '/sign-up'
    },
  },
  methods: {
    // validate the form
    validateForm(userInfo) {
      return this.$refs.form.validate()
        ? // Emit an action event
          this.$emit('form-action', userInfo)
        : (this.errorInfo = 'Error, some important fields are missing')
      // Triger a notification for the error
    },
  },
}
</script>

<style lang="scss" scoped></style>
