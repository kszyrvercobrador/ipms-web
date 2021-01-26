<template>
  <v-app>
    <v-app-bar fixed app dark color="primary">
      <v-toolbar-title> IP Address Management Solution </v-toolbar-title>
      <v-spacer />
      <v-btn text @click="logout">
        <v-icon left> mdi-logout </v-icon>
        Logout
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  middleware: 'auth',

  methods: {
    logout() {
      this.$auth.logout()
    },
  },
}
</script>
