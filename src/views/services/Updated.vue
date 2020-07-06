<template>
  <gov-width-container>
    <ck-loader v-if="loading" />
    <template v-else>
      <vue-headful :title="`${appName} - Service Updated: ${service.name}`" />

      <gov-back-link :to="{ name: 'services-show', params: { service: service.id } }">Back to {{ service.type }}</gov-back-link>
      <gov-main-wrapper>
        <gov-grid-row>
          <gov-grid-column width="one-half">
            <gov-heading size="xl">Update request submitted</gov-heading>
            <gov-body>
              Thank you for submitting your update, the LOOP team will review and approve your record within 5-7 working days. If you have any problems or queries, please contact <gov-link :href="`mailto:info@looprepository.org`">info@looprepository.org</gov-link>.
            </gov-body>

            <gov-button :to="{ name: 'services-show', params: { service: this.$route.params.service } }">Back to {{ service.type }}</gov-button>
          </gov-grid-column>
        </gov-grid-row>
      </gov-main-wrapper>
    </template>
  </gov-width-container>
</template>

<script>
import http from "@/http";

export default {
  name: "ServiceUpdated",
  data() {
    return {
      loading: false,
      service: null
    };
  },
  methods: {
    async fetchService() {
      this.loading = true;
      const response = await http.get(
        `/services/${this.$route.params.service}`
      );
      this.service = response.data.data;
      this.loading = false;
    }
  },
  created() {
    this.fetchService();
  }
};
</script>
