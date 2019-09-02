<template>
  <gov-width-container>
    <ck-loader v-if="loading" />
    <template v-else>
      <vue-headful
        :title="`${appName} - Edit SNOMED Collection: ${collection.name}`"
      />

      <gov-back-link :to="{ name: 'admin-index-collections-snomed' }">
        Back to SNOMED collections
      </gov-back-link>

      <gov-main-wrapper>
        <gov-grid-row>
          <gov-grid-column width="one-half">
            <gov-heading size="xl">
              <gov-caption size="xl">Collections</gov-caption>
              SNOMED
            </gov-heading>

            <gov-heading size="m">Edit SNOMED code</gov-heading>

            <gov-body>
              From this page, you can edit the SNOMED codes that appear on
              the homepage. You can change which taxonomies they refer to, the
              icon used, and the information provided in the description and
              sidebox.
            </gov-body>

            <collection-form
              :errors="form.$errors"
              :code.sync="form.code"
              :name.sync="form.name"
              :order.sync="form.order"
              :category_taxonomies.sync="form.category_taxonomies"
              @clear="form.$errors.clear($event)"
            />

            <gov-button v-if="form.$submitting" disabled type="submit">
              Updating...
            </gov-button>

            <gov-button v-else @click="onSubmit" type="submit">
              Update
            </gov-button>

            <ck-submit-error v-if="form.$errors.any()" />

            <gov-section-break size="l" />

            <ck-delete-button
              resource="SNOMED code"
              :endpoint="`/collections/snomed/${this.collection.id}`"
              @deleted="onDelete"
            />
          </gov-grid-column>
        </gov-grid-row>
      </gov-main-wrapper>
    </template>
  </gov-width-container>
</template>

<script>
import http from "@/http";
import Form from "@/classes/Form";
import CollectionForm from "@/views/collections/snomed/forms/CollectionForm";

export default {
  name: "EditSnomedCollection",
  components: { CollectionForm },
  data() {
    return {
      loading: false,
      collection: null,
      form: null
    };
  },
  methods: {
    async fetchCollection() {
      this.loading = true;

      const response = await http.get(
        `/collections/snomed/${this.$route.params.collection}`
      );
      this.collection = response.data.data;
      this.form = new Form({
        code: this.collection.code,
        name: this.collection.name,
        order: this.collection.order,
        category_taxonomies: this.collection.category_taxonomies.map(
          taxonomy => taxonomy.id
        )
      });

      this.loading = false;
    },
    async onSubmit() {
      await this.form.put(`/collections/snomed/${this.collection.id}`);
      this.$router.push({ name: "admin-index-collections-snomed" });
    },
    onDelete() {
      this.$router.push({ name: "admin-index-collections-snomed" });
    }
  },
  created() {
    this.fetchCollection();
  }
};
</script>
