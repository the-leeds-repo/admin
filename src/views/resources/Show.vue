<template>
  <gov-width-container>
    <gov-back-link :to="{ name: 'resources-index' }">
      Back to resources
    </gov-back-link>

    <gov-main-wrapper>
      <ck-loader v-if="loading" />

      <gov-grid-row v-else>
        <vue-headful :title="`${appName} - Resource: ${resource.name}`" />

        <gov-grid-column width="two-thirds">
          <gov-heading size="m">View resource</gov-heading>

          <ck-resource-details :resource="resource" />

          <template v-if="auth.isSuperAdmin">
            <gov-body>
              Please be certain of the action before deleting a resource
            </gov-body>

            <gov-section-break size="l" />

            <ck-delete-button
              resource="resource"
              :endpoint="`/resources/${this.resource.id}`"
              @deleted="onDelete"
            />
          </template>
        </gov-grid-column>

        <gov-grid-column
          v-if="auth.isOrganisationAdmin(resource.organisation)"
          width="one-third"
          class="text-right"
        >
          <gov-button
            :to="{
              name: 'resources-edit',
              params: { resource: resource.id }
            }"
          >
            Edit resource
          </gov-button>
        </gov-grid-column>
      </gov-grid-row>
    </gov-main-wrapper>
  </gov-width-container>
</template>

<script>
import http from "@/http";
import CkResourceDetails from "@/components/CkResourceDetails.vue";

export default {
  name: "ShowResource",

  components: {
    CkResourceDetails
  },

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
      } = await http.get(
        `/resources/${this.$route.params.resource}?include=organisation`
      );

      this.resource = resource;

      this.loading = false;
    },

    onDelete() {
      this.$router.push({ name: "resources-index" });
    }
  },

  created() {
    this.fetchResource();
  }
};
</script>
