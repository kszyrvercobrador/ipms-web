<template>
  <div class="full-page">
    <v-card class="py-10 px-6">
      <v-card-title class="text-h3 text-center"> Login </v-card-title>
      <v-card-subtitle class="mb-6">
        Enter your credentials below to continue
      </v-card-subtitle>
      <v-card-text>
        <v-form class="login-form" @submit.prevent="submit">
          <v-text-field
            v-model="form.email"
            label="Email"
            type="email"
            outlined
          />
          <v-text-field
            v-model="form.password"
            label="Password"
            type="password"
            outlined
          />
          <div class="d-flex flex-column justify-center">
            <v-btn
              depressed
              color="primary"
              class="px-12 mb-2"
              large
              type="submit"
            >
              Login
            </v-btn>
            <v-btn to="/auth/register" text color="primary" class="px-12" large>
              Don't have account ? Sign up
            </v-btn>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import Form from '@/utils/form'

export default {
  layout: 'auth',

  data() {
    return {
      form: new Form({
        email: '',
        password: '',
      }),
    }
  },

  methods: {
    submit() {
      this.$auth.loginWith('local', {
        data: this.form.$data(),
      })
    },
  },
}
</script>

<style lang="scss">
.login-form {
  width: 100%;
  min-width: 300px;
}
</style>
