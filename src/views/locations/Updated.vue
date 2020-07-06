<template>
  <gov-width-container>
    <ck-loader v-if="loading" />
    <template v-else>
      <vue-headful :title="`${appName} - Location Updated: ${location.address_line_1}`" />

      <gov-back-link :to="{ name: 'locations-show', params: { location: location.id } }">Back to location</gov-back-link>
      <gov-main-wrapper>
        <gov-grid-row>
          <gov-grid-column width="one-half">
            <gov-heading size="xl">Update request submitted</gov-heading>
            <gov-body>
              Thank you for submitting your update, the LOOP team will review and approve your record within 5-7 working days. If you have any problems or queries, please contact <gov-link :href="`mailto:info@looprepository.org`">info@looprepository.org</gov-link>.
            </gov-body>

            <gov-button :to="{ name: 'locations-show', params: { location: this.$route.params.location } }">Back to location</gov-button>
          </gov-grid-column>
        </gov-grid-row>
      </gov-main-wrapper>
    </template>
  </gov-width-container>
</template>

<script>
import http from "@/http";

export default {
  name: "LocationUpdated",
  data() {
    return {
      loading: false,
      location: null
    };
  },
  methods: {
    async fetchLocation() {
      this.loading = true;
      const response = await http.get(
        `/locations/${this.$route.params.location}`
      );
      this.location = response.data.data;
      this.loading = false;
    }
  },
  created() {
    this.fetchLocation();
  }
};
</script>
