<template>
  <gov-table>
    <template slot="header">
      <gov-table-row>
        <gov-table-header scope="col">Organisation</gov-table-header>
        <gov-table-header scope="col">Status Code</gov-table-header>
        <gov-table-header scope="col">Date / Time</gov-table-header>
        <gov-table-header scope="col" right></gov-table-header>
      </gov-table-row>
    </template>
    <template slot="body">
      <gov-table-row v-for="failedCiviSync in failedCiviSyncs" :key="failedCiviSync.id">
        <gov-table-cell>
          <gov-link :to="{ name: 'organisations-show', params: { organisation: failedCiviSync.organisation_id } }">{{ failedCiviSync.organisation.name }}</gov-link>
        </gov-table-cell>

        <gov-table-cell>
          <gov-tag>{{ failedCiviSync.status_code }}</gov-tag>
        </gov-table-cell>

        <gov-table-cell>{{ formatCreatedAt(failedCiviSync) }}</gov-table-cell>

        <gov-table-cell right>
          <gov-link v-if="!retrying" @click="onRetry(failedCiviSync)">Retry</gov-link>
          <template v-else>Retrying</template>
        </gov-table-cell>
      </gov-table-row>
      <gov-table-row v-if="failedCiviSyncs.length === 0">
        <gov-table-cell center colspan="4">No failed syncs</gov-table-cell>
      </gov-table-row>
    </template>
  </gov-table>
</template>

<script>
import http from "@/http";

export default {
  name: "CkFailedCiviSynsTable",

  props: {
    failedCiviSyncs: {
      type: Array,
      required: true
    }
  },

  data() {
    return {
      retrying: false
    }
  },

  methods: {
    formatCreatedAt(failedCiviSync) {
      return this.formatDateTime(failedCiviSync.created_at);
    },

    async onRetry(failedCiviSync) {
      this.retrying = true

      try {
        await http.post(`/failed-civi-syncs/${failedCiviSync.id}/retry`)
      } catch (error) {
        window.alert('Failed to retry sync')
      }

      this.retrying = false

      this.$emit('retry')
    }
  }
};
</script>
