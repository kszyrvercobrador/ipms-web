<template>
  <v-container>
    <v-toolbar flat>
      <v-toolbar-title> Activity Logs </v-toolbar-title>
    </v-toolbar>

    <v-simple-table>
      <v-card class="ma-4" :loading="loading">
        <v-simple-table>
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Log Description</th>
                <th class="text-left">Causer</th>
                <th class="text-left">Subject</th>
                <th>Timestamp</th>
                <th width="1%">Properties</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="log in logs" :key="log.id">
                <td class="text-uppercase">{{ log.description }}</td>
                <td>{{ log.causer ? log.causer.name : '' }}</td>
                <td>{{ getSubjectName(log.subject_type) }}</td>
                <td>{{ log.created_at }}</td>
                <td>
                  <v-dialog width="500">
                    <template v-slot:activator="{ on }">
                      <v-btn text v-on="on"> Show Changes </v-btn>
                    </template>

                    <v-card>
                      <v-card-title class="headline grey lighten-2">
                        Log details
                      </v-card-title>

                      <v-card-text>
                        <pre>
                         {{ log.properties }}
                       </pre
                        >
                      </v-card-text>
                    </v-card>
                  </v-dialog>
                </td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-card>
    </v-simple-table>
    <v-pagination
      v-model="meta.current_page"
      :length="meta.length"
    ></v-pagination>
  </v-container>
</template>

<script>
export default {
  middleware: 'auth',

  data() {
    return {
      loading: false,
      logs: [],
      meta: {
        current_page: 1,
        last_page: 1,
        length: 1,
      },
    }
  },

  created() {
    this.fetchLogs()
  },

  methods: {
    fetchLogs() {
      this.loading = true
      this.$axios
        .get('/api/activity-logs?page=' + this.meta.current_page)
        .then((res) => {
          this.logs = res.data.data
          this.meta.current_page = res.data.current_page
          this.meta.last_page = res.data.last_page
          this.meta.length = Math.ceil(res.data.total / res.data.per_page)
          this.loading = false
        })
    },
    getSubjectName(type) {
      const name = String(type).split('\\').slice(-1)[0]
      if (name && name != 'null') {
        return name
      }
    },
  },

  watch: {
    'meta.current_page'() {
      this.fetchLogs()
    },
  },
}
</script>

<style>
</style>
