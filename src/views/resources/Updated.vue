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
              Your update request for this resource has been received. It will
              need to be approved by an admin before the changes will be
              applied.
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

      const { data: { data: resource } } = await http.get(
        `/resources/${this.$route.params.resource}`
      );
      this.resource = resource;

      this.loading = false;
    }
  },

  created() {
    this.fetchResource();
  }
};
</script>
