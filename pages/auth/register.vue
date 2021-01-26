<template>
  <div class="full-page">
    <v-card class="py-10 px-6">
      <v-card-title class="text-h3 text-center"> Register </v-card-title>
      <v-card-subtitle class="mb-6">
        Enter your details below to continue
      </v-card-subtitle>
      <v-card-text>
        <v-form class="register-form" @submit.prevent="register">
          <v-text-field
            v-model="form.name"
            :error-messages="form.$getError('name')"
            label="Name"
            outlined
          />
          <v-text-field
            v-model="form.email"
            :error-messages="form.$getError('email')"
            label="Email"
            type="email"
            outlined
          />
          <v-text-field
            v-model="form.password"
            :error-messages="form.$getError('password')"
            label="Password"
            type="password"
            outlined
          />
          <v-text-field
            v-model="form.password_confirmation"
            label="Confirm Password"
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
              :loading="form.$busy"
            >
              Sign up
            </v-btn>
            <v-btn
              to="/auth/login"
              text
              color="primary"
              class="px-12"
              large
              :disabled="form.$busy"
            >
              Already have account ? Sign in
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
  auth: 'guest',

  data() {
    return {
      form: new Form({
        name: '',
        email: '',
        password: '',
        password_confirmation: '',
      }),
    }
  },

  methods: {
    register() {
      this.form.$clearErrors()
      this.form.$busy = true
      this.$axios
        .post('/api/auth/register', this.form.$data())
        .then((res) => {
          this.$auth
            .loginWith('local', {
              data: { email: this.form.email, password: this.form.password },
            })
            .then((this.form.$busy = false))
        })
        .catch((err) => {
          this.form.$busy = false
          if (err.response && err.response.status == 422) {
            this.form.$setErrors(err.response.data.errors)
          }
        })
    },
  },

  mounted() {
    if (this.$auth.loggedIn) {
      this.$router.replace('/')
    }
  },
}
</script>

<style lang="scss">
.register-form {
  width: 100%;
  min-width: 300px;
}
</style>
