<template>
  <gov-width-container>
    <vue-headful :title="`${appName} - List Services`" />

    <gov-back-link :to="{ name: 'dashboard' }">Back to dashboard</gov-back-link>

    <gov-main-wrapper>
      <gov-grid-row>
        <gov-grid-column width="full">

          <gov-heading size="xl">Services</gov-heading>

          <gov-grid-row>
            <gov-grid-column width="two-thirds">
              <ck-table-filters @search="onSearch">
                <gov-form-group>
                  <gov-label for="filter[name]" optional>Service name</gov-label>
                  <gov-input v-model="filters.name" id="filter[name]" name="filter[name]" type="search"/>
                </gov-form-group>

                <gov-form-group>
                  <gov-label for="filter[postcode]" optional>Postcode</gov-label>
                  <gov-input v-model="filters.postcode" id="filter[postcode]" name="filter[postcode]" type="search"/>
                </gov-form-group>

                <template slot="extra-filters">
                  <gov-form-group>
                    <gov-label for="filter[organisation_name]" optional>Organisation name</gov-label>
                    <gov-input v-model="filters.organisation_name" id="filter[organisation_name]" name="filter[organisation_name]" type="search"/>
                  </gov-form-group>

                  <gov-form-group>
                    <gov-label for="filter[status]" optional>Status</gov-label>
                    <gov-select v-model="filters.status" id="filter[status]" name="filter[status]" :options="statuses"/>
                  </gov-form-group>

                  <gov-form-group>
                    <gov-label for="filter[status]" optional>Taxonomies</gov-label>
                    <category-taxonomy-input
                      v-model="filters.taxonomy_id"
                      value-consists-of="LEAF_PRIORITY"
                      :error="false"
                    />
                  </gov-form-group>
                </template>
              </ck-table-filters>
            </gov-grid-column>
            <gov-grid-column v-if="auth.isOrganisationAdmin()" width="one-third">
              <gov-button @click="onAddService" type="submit" success expand>Add service</gov-button>
            </gov-grid-column>
          </gov-grid-row>

          <ck-resource-listing-table
            ref="servicesTable"
            uri="/services"
            :params="params"
            default-sort="name"
            :columns="[
              { heading: 'Service name', sort: 'name', render: (service) => service.name },
              { heading: 'Organisation', sort: 'organisation_name', render: (service) => service.organisation.name },
              { heading: 'Status', render: (service) => displayStatus(service.status) },
              { heading: 'Freshness', sort: 'last_modified_at', render: (service) => displayFreshness(service.last_modified_at) },
            ]"
            :view-route="(service) => {
              return {
                name: 'services-show',
                params: { service: service.id }
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
import CategoryTaxonomyInput from "@/views/services/inputs/CategoryTaxonomyInput";

export default {
  name: "ListServices",
  components: { CkResourceListingTable, CkTableFilters, CategoryTaxonomyInput },
  data() {
    return {
      filters: {
        name: "",
        postcode: "",
        organisation_name: "",
        status: "",
        taxonomy_id: []
      },
      statuses: [
        { value: "", text: "All" },
        { value: "active", text: "Enabled" },
        { value: "inactive", text: "Disabled" }
      ]
    };
  },
  computed: {
    params() {
      const params = {
        include: "organisation"
      };

      if (this.filters.name !== "") {
        params["filter[name]"] = this.filters.name;
      }

      if (this.filters.postcode !== "") {
        params["filter[postcode]"] = this.filters.postcode;
      }

      if (this.filters.organisation_name !== "") {
        params["filter[organisation_name]"] = this.filters.organisation_name;
      }

      if (this.filters.status !== "") {
        params["filter[status]"] = this.filters.status;
      }

      if (this.filters.taxonomy_id.length > 0) {
        params["filter[taxonomy_id]"] = this.filters.taxonomy_id.join(",");
      }

      return params;
    }
  },
  methods: {
    onSearch() {
      this.$refs.servicesTable.currentPage = 1;
      this.$refs.servicesTable.fetchResources();
    },
    onAddService() {
      this.$router.push({ name: "services-pre-create-1" });
    },
    displayStatus(status) {
      switch (status) {
        case "active":
          return "Enabled";
        case "inactive":
          return "Disabled";
        default:
          return status;
      }
    },
    displayFreshness(lastModifiedAt) {
      const start = this.moment(lastModifiedAt, this.moment.ISO_8601);
      const end = this.moment();
      const difference = end.diff(start, "months");
      let title = `last updated ${difference} months ago`;

      if (difference === 0) {
        title = 'last updated this month';
      } else if (difference === 1) {
        title = 'last updated a month ago';
      }

      if (difference >= 6) {
        return `<div class="app-freshness app-freshness--old" title="Old (${title})"></div> ${start.format('D/M/YYYY')}`;
      } else if (difference >= 3) {
        return `<div class="app-freshness app-freshness--stale" title="Stale (${title})"></div> ${start.format('D/M/YYYY')}`;
      }

      return `<div class="app-freshness app-freshness--fresh" title="Fresh (${title})"></div> ${start.format('D/M/YYYY')}`;
    },
    displayReferralMethod(referralMethod) {
      return referralMethod.charAt(0).toUpperCase() + referralMethod.substr(1);
    }
  }
};
</script>

<style lang="scss">
.app-freshness {
  width: 1rem;
  height: 1rem;
  border-radius: 100%;

  &.app-freshness--old {
    background-color: red;
  }

  &.app-freshness--stale {
    background-color: orange;
  }

  &.app-freshness--fresh {
    background-color: green;
  }
}
</style>
