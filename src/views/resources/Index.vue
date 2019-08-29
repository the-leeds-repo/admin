<template>
  <gov-width-container>
    <vue-headful :title="`${appName} - List Resources`" />

    <gov-back-link :to="{ name: 'dashboard' }">Back to dashboard</gov-back-link>

    <gov-main-wrapper>
      <gov-grid-row>
        <gov-grid-column width="full">

          <gov-heading size="xl">Resources</gov-heading>

          <gov-grid-row>
            <gov-grid-column width="two-thirds">
              <ck-table-filters @search="onSearch">
                <gov-form-group>
                  <gov-label for="filter[name]">Resource Name</gov-label>
                  <gov-input v-model="filters.name" id="filter[name]" name="filter[name]" type="search"/>
                </gov-form-group>

                <template slot="extra-filters">
                  <gov-form-group>
                    <gov-label for="filter[organisation_name]">Organisation Name</gov-label>
                    <gov-input v-model="filters.organisation_name" id="filter[organisation_name]" name="filter[organisation_name]" type="search"/>
                  </gov-form-group>
                </template>
              </ck-table-filters>
            </gov-grid-column>
            <gov-grid-column v-if="auth.isOrganisationAdmin()" width="one-third">
              <gov-button @click="onAddResource" type="submit" success expand>Add resource</gov-button>
            </gov-grid-column>
          </gov-grid-row>

          <ck-resource-listing-table
            ref="resourcesTable"
            uri="/resources"
            :params="params"
            default-sort="name"
            :columns="[
              { heading: 'Resource name', sort: 'name', render: (resource) => resource.name },
              { heading: 'Organisation', sort: 'organisation_name', render: (resource) => resource.organisation.name },
              { heading: 'Published date', render: (resource) => formatDate(resource.published_at) },
            ]"
            :view-route="(resource) => {
              return {
                name: 'resources-show',
                params: { resource: resource.id }
              }
            }"
          />
        </gov-grid-column>
      </gov-grid-row>
    </gov-main-wrapper>
  </gov-width-container>
</template>

<script>
import CkResourceListingTable from "@/components/Ck/CkResourceListingTable.vue";
import CkTableFilters from "@/components/Ck/CkTableFilters.vue";

export default {
  name: "ListResources",
  components: { CkResourceListingTable, CkTableFilters },
  data() {
    return {
      filters: {
        name: "",
        organisation_name: ""
      }
    };
  },
  computed: {
    params() {
      let params = {};

      if (this.filters.name !== "") {
        params["filter[name]"] = this.filters.name;
      }

      if (this.filters.organisation_name !== "") {
        params["filter[organisation_name]"] = this.filters.organisation_name;
      }

      return params;
    }
  },
  methods: {
    onSearch() {
      this.$refs.resourcesTable.currentPage = 1;
      this.$refs.resourcesTable.fetchResources();
    },
    onAddResource() {
      this.$router.push({ name: "resources-create" });
    }
  }
};
</script>
