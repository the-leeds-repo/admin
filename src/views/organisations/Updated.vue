<template>
  <gov-width-container>
    <ck-loader v-if="loading" />
    <template v-else>
      <vue-headful :title="`${appName} - Organisation Updated: ${organisation.name}`" />

      <gov-back-link :to="{ name: 'organisations-show', params: { organisation: organisation.id } }">Back to organisation</gov-back-link>
      <gov-main-wrapper>
        <gov-grid-row>
          <gov-grid-column width="one-half">
            <gov-heading size="xl">Update request submitted</gov-heading>
            <gov-body>
              Thank you for submitting your update, the LOOP team will review and approve your record within 5-7 working days. If you have any problems or queries, please contact <gov-link :href="`mailto:info@looprepository.org`">info@looprepository.org</gov-link>.
            </gov-body>

            <gov-button :to="{ name: 'organisations-show', params: { organisation: this.$route.params.organisation } }">Back to organisation</gov-button>
          </gov-grid-column>
        </gov-grid-row>
      </gov-main-wrapper>
    </template>
  </gov-width-container>
</template>

<script>
import http from "@/http";

export default {
  name: "OrganisationUpdated",
  data() {
    return {
      loading: false,
      organisation: null
    };
  },
  methods: {
    async fetchOrganisation() {
      this.loading = true;
      const response = await http.get(
        `/organisations/${this.$route.params.organisation}`
      );
      this.organisation = response.data.data;
      this.loading = false;
    }
  },
  created() {
    this.fetchOrganisation();
  }
};
</script>
