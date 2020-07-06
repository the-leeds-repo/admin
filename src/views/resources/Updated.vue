<template>
  <gov-width-container>
    <ck-loader v-if="loading" />
    <template v-else>
      <vue-headful :title="`${appName} - Resource Updated: ${resource.name}`" />

      <gov-back-link
        :to="{
          name: 'resources-show',
          params: { resource: resource.id }
        }"
      >
        Back to resource
      </gov-back-link>
      <gov-main-wrapper>
        <gov-grid-row>
          <gov-grid-column width="one-half">
            <gov-heading size="xl">Update request submitted</gov-heading>
            <gov-body>
              Thank you for submitting your update, the LOOP team will review and approve your record within 5-7 working days. If you have any problems or queries, please contact <gov-link :href="`mailto:info@looprepository.org`">info@looprepository.org</gov-link>.
            </gov-body>

            <gov-button
              :to="{
                name: 'resources-show',
                params: { resource: this.$route.params.resource }
              }"
            >
              Back to resource
            </gov-button>
          </gov-grid-column>
        </gov-grid-row>
      </gov-main-wrapper>
    </template>
  </gov-width-container>
</template>

<script>
import http from "@/http";

export default {
  name: "ResourceUpdated",

  data() {
    return {
      loading: false,
      resource: null
    };
  },

  methods: {
    async fetchResource() {
      this.loading = true;

      const {
        data: { data: resource }
      } = await http.get(`/resources/${this.$route.params.resource}`);
      this.resource = resource;

      this.loading = false;
    }
  },

  created() {
    this.fetchResource();
  }
};
</script>
