<template>
  <v-container>
    <v-toolbar flat>
      <v-toolbar-title> IP Address </v-toolbar-title>
      <v-spacer />
      <v-btn color="primary" @click="showIpForm">
        <v-icon left> mdi-plus </v-icon>
        Add new record
      </v-btn>
    </v-toolbar>

    <v-card class="ma-4" :loading="loading">
      <v-simple-table>
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Label</th>
              <th class="text-left">IP Address</th>
              <th width="1%"></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in items" :key="item.id">
              <td>{{ item.label }}</td>
              <td>{{ item.ip_address }}</td>
              <td>
                <v-btn icon @click="edit(item)"
                  ><v-icon>mdi-pencil</v-icon></v-btn
                >
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </v-card>

    <v-dialog v-model="showIpFormDialog" width="500px">
      <ip-form
        @cancel="showIpFormDialog = false"
        @added="updateItems"
        @updated="updateItems"
        :ip-address="item"
      />
    </v-dialog>
  </v-container>
</template>

<script>
import IpForm from '@/components/ip-form'
import { findIndex } from 'lodash'

export default {
  middleware: 'auth',
  components: {
    IpForm,
  },

  data() {
    return {
      showIpFormDialog: false,
      item: null,
      items: [],
      loading: false,
    }
  },

  created() {
    this.fetchItems()
  },

  methods: {
    fetchItems() {
      this.loading = true
      this.$axios.get('/api/ip-address').then((res) => {
        this.items = res.data.data
        this.loading = false
      })
    },
    showIpForm() {
      this.item = null
      this.showIpFormDialog = true
    },
    edit(item) {
      this.item = item
      this.showIpFormDialog = true
    },
    updateItems(item) {
      this.showIpFormDialog = false
      const i = findIndex(this.items, { id: item.id })
      if (i >= 0) {
        this.items.splice(i, 1, item)
      } else {
        this.items.unshift(item)
      }
    },
  },
}
</script>
