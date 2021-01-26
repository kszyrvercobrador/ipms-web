<template>
  <v-card>
    <v-toolbar class="mb-8" color="primary" dark dense>
      <v-toolbar-title v-if="!ipAddress"> Add New IP Address </v-toolbar-title>
      <v-toolbar-title v-if="ipAddress"> Update IP Address </v-toolbar-title>
    </v-toolbar>
    <v-card-text>
      <v-form @submit.prevent="submit">
        <v-text-field
          v-model="form.label"
          :error-messages="form.$getError('label')"
          label="Label"
          outlined
        />
        <v-text-field
          v-model="form.ip_address"
          :error-messages="form.$getError('ip_address')"
          :disabled="!!ipAddress"
          label="IP Address"
          outlined
        />
        <div class="mt-4 d-flex justify-space-between">
          <v-btn @click="cancel" :disabled="form.$busy">Cancel</v-btn>
          <v-btn color="primary" type="submit" :loading="form.$busy"
            >Submit</v-btn
          >
        </div>
      </v-form>
    </v-card-text>
  </v-card>
</template>

<script>
import Form from '@/utils/form'

export default {
  props: {
    ipAddress: {
      type: Object,
      required: false,
    },
  },

  data() {
    return {
      form: new Form({
        label: '',
        ip_address: '',
      }),
    }
  },

  created() {
    this.setFormFromIpAddress()
  },

  methods: {
    submit() {
      if (this.ipAddress) {
        this.updateIpAddress()
      } else {
        this.createNew()
      }
    },

    setFormFromIpAddress() {
      if (this.ipAddress) {
        this.form = new Form({
          label: this.ipAddress.label,
          ip_address: this.ipAddress.ip_address,
        })
      } else {
        this.form = new Form({
          label: '',
          ip_address: '',
        })
      }
    },

    createNew() {
      this.form.$busy = true
      this.form.$clearErrors()
      this.$axios
        .post('/api/ip-address', this.form.$data())
        .then((res) => {
          this.form.$busy = false
          this.form.$reset()
          this.$emit('added', res.data.data)
        })
        .catch((err) => {
          this.form.$busy = false
          if (err.response && err.response.status == 422) {
            this.form.$setErrors(err.response.data.errors)
          }
        })
    },

    updateIpAddress() {
      this.form.$busy = true
      this.form.$clearErrors()
      this.$axios
        .put('/api/ip-address/' + this.ipAddress.id, {
          label: this.form.label,
        })
        .then((res) => {
          this.form.$busy = false
          this.form.$reset()
          this.$emit('added', res.data.data)
        })
        .catch((err) => {
          this.form.$busy = false
          if (err.response && err.response.status == 422) {
            this.form.$setErrors(err.response.data.errors)
          }
        })
    },

    cancel() {
      this.form.$reset()
      this.$emit('cancel')
    },
  },

  watch: {
    ipAddress() {
      this.setFormFromIpAddress()
    },
  },
}
</script>

<style>
</style>
