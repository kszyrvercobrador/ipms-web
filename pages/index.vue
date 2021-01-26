<template>
  <v-toolbar flat>
    <v-toolbar-title> IP Address </v-toolbar-title>
    <v-spacer />
    <v-btn color="primary">
      <v-icon left> mdi-plus </v-icon>
      Add new record
    </v-btn>
  </v-toolbar>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  middleware: 'auth',
  components: {
    Logo,
    VuetifyLogo,
  },
}
</script>
