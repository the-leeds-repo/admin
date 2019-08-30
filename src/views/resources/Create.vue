<template>
  <gov-width-container>
    <vue-headful :title="`${appName} - Add Resource`" />

    <gov-back-link :to="{ name: 'resources-index' }">Back to resources</gov-back-link>
    <gov-main-wrapper>
      <gov-grid-row>
        <gov-grid-column width="one-half">
          <gov-heading size="xl">Resources</gov-heading>
          <gov-heading size="m">Add resource</gov-heading>

          <resource-form
            :errors="form.$errors"
            :organisation_id.sync="form.organisation_id"
            :name.sync="form.name"
            :slug.sync="form.slug"
            :description.sync="form.description"
            :url.sync="form.url"
            :license.sync="form.license"
            :author.sync="form.author"
            :category_taxonomies.sync="form.category_taxonomies"
            :published_at.sync="form.published_at"
            :last_modified_at.sync="form.last_modified_at"
            @clear="form.$errors.clear($event)"
          />

          <gov-button v-if="form.$submitting" disabled type="submit">Creating...</gov-button>
          <gov-button v-else @click="onSubmit" type="submit">Create</gov-button>
          <ck-submit-error v-if="form.$errors.any()" />
        </gov-grid-column>
      </gov-grid-row>
    </gov-main-wrapper>
  </gov-width-container>
</template>

<script>
import Form from "@/classes/Form";
import ResourceForm from "@/views/resources/forms/ResourceForm";

export default {
  name: "CreateResource",
  components: { ResourceForm },
  data() {
    return {
      form: new Form({
        organisation_id: "",
        name: "",
        slug: "",
        description: "",
        url: "",
        license: "",
        author: "",
        category_taxonomies: [],
        published_at: "",
        last_modified_at: ""
      })
    };
  },
  methods: {
    async onSubmit() {
      const {
        data: { id }
      } = await this.form.post("/resources");

      this.$router.push({
        name: "resources-show",
        params: { resource: id }
      });
    }
  }
};
</script>
