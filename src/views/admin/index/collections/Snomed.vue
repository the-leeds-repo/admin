<template>
  <div>
    <gov-grid-row>
      <gov-grid-column width="two-thirds">
        <gov-heading size="l">Collection: SNOMED</gov-heading>

        <gov-body>
          On this page, you can select which taxonomies apply for each SNOMED
          code on the front page. You can also add SNOMED codes using the button
          above.
        </gov-body>

        <gov-body>
          You can move SNOMED codes up and down. This denotes the order in which
          the SNOMED codes appear on the home page. The SNOMED code at the top
          of the list will appear on the upper-leftmost corner.
        </gov-body>
      </gov-grid-column>

      <gov-grid-column v-if="auth.isSuperAdmin" width="one-third">
        <gov-button
          :to="{ name: 'collections-snomed-create' }"
          success
          expand
        >
          Add a new SNOMED code
        </gov-button>
      </gov-grid-column>
    </gov-grid-row>

    <gov-section-break size="l" />

    <!-- Loop through each category -->
    <gov-grid-row>
      <gov-grid-column width="two-thirds">

        <ck-loader v-if="loading" />
        <gov-list v-else bullet>
          <li v-for="collection in collections" :key="collection.id">
            <strong>{{ collection.code }}:</strong>
            {{ collection.name || 'N/A' }}
            <!---->&nbsp;<!---->
            <gov-link
              v-if="auth.isGlobalAdmin"
              :to="{
                name: 'collections-snomed-edit',
                params: { collection: collection.id }
              }"
            >Edit</gov-link>

            <br>

            <gov-link
              @click="onMoveUp(collection)"
              v-if="collection.order > 1"
            >(Move up)</gov-link>
            <!---->&nbsp;<!---->
            <gov-link
              @click="onMoveDown(collection)"
              v-if="collection.order < collections.length"
            >(Move down)</gov-link>
          </li>
        </gov-list>

      </gov-grid-column>
    </gov-grid-row>
  </div>
</template>

<script>
import http from "@/http";

export default {
  name: "ListCollectionSnomed",

  data() {
    return {
      loading: false,
      collections: []
    };
  },

  methods: {
    async fetchCollections() {
      this.loading = true;
      this.collections = await this.fetchAll("/collections/snomed");
      this.loading = false;
    },

    async onMoveUp(collection) {
      this.loading = true;

      await http.put(`/collections/snomed/${collection.id}`, {
        ...this.parseCollectionForUpdate(collection),
        order: collection.order - 1
      });

      this.fetchCollections();
    },

    async onMoveDown(collection) {
      this.loading = true;

      await http.put(`/collections/snomed/${collection.id}`, {
        ...this.parseCollectionForUpdate(collection),
        order: collection.order + 1
      });

      this.fetchCollections();
    },

    parseCollectionForUpdate(collection) {
      return {
        code: collection.code,
        name: collection.name,
        order: collection.order,
        category_taxonomies: collection.category_taxonomies.map(
          taxonomy => taxonomy.id
        )
      };
    }
  },

  created() {
    this.fetchCollections();
  }
};
</script>
