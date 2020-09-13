<template>
  <div>
    <vue-headful :title="`${appName} - Admin: CiviCRM`" />

    <gov-heading size="l">CiviCRM</gov-heading>
    <ck-loader v-if="loading" />
    <template v-else>
      <ck-failed-civi-syncs-table :failed-civi-syncs="failedCiviSyncs" @retry="onRetry" />
      <gov-body>
        Page {{ currentPage }} of {{ lastPage }}
        <gov-link v-if="currentPage > 1" @click="onPrevious">Back</gov-link>&nbsp;<!--
     --><gov-link v-if="currentPage < lastPage" @click="onNext">Next</gov-link>
      </gov-body>
    </template>
  </div>
</template>

<script>
import http from "@/http";
import CkFailedCiviSyncsTable from "@/components/CkFailedCiviSyncsTable";

export default {
  name: "ListCiviCRM",
  components: { CkFailedCiviSyncsTable },
  data() {
    return {
      loading: false,
      failedCiviSyncs: [],
      currentPage: 1,
      lastPage: 1
    };
  },
  methods: {
    async fetchFailedCiviSyncs() {
      this.loading = true;

      const { data } = await http.get("/failed-civi-syncs", {
        params: {
          page: this.currentPage,
          include: 'organisation'
        }
      });
      this.failedCiviSyncs = data.data;
      this.currentPage = data.meta.current_page;
      this.lastPage = data.meta.last_page;

      this.loading = false;
    },
    onNext() {
      this.currentPage++;
      this.fetchFailedCiviSyncs();
    },
    onPrevious() {
      this.currentPage--;
      this.fetchFailedCiviSyncs();
    },
    onRetry() {
      this.currentPage = 1;
      this.fetchFailedCiviSyncs();
    }
  },
  created() {
    this.fetchFailedCiviSyncs();
  }
};
</script>
