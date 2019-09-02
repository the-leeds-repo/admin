<template>
  <gov-width-container>
    <vue-headful :title="`${appName} - Add SNOMED Collection`" />

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

          <gov-heading size="m">Add SNOMED code</gov-heading>

          <gov-body>
            From this page, you can add the SNOMED codes that appear on the
            homepage. You can specify which taxonomies they refer to, the icon
            used, and the information provided in the description and sidebox.
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
            Creating...
          </gov-button>

          <gov-button v-else @click="onSubmit" type="submit">
            Create
          </gov-button>

          <ck-submit-error v-if="form.$errors.any()" />
        </gov-grid-column>
      </gov-grid-row>
    </gov-main-wrapper>
  </gov-width-container>
</template>

<script>
import CollectionForm from "@/views/collections/snomed/forms/CollectionForm";
import Form from "@/classes/Form";

export default {
  name: "CreateSnomedCollection",
  components: { CollectionForm },
  data() {
    return {
      form: new Form({
        code: "",
        name: "",
        order: 1,
        category_taxonomies: []
      })
    };
  },
  methods: {
    async onSubmit() {
      await this.form.post("/collections/snomed");
      this.$router.push({ name: "admin-index-collections-snomed" });
    }
  }
};
</script>
