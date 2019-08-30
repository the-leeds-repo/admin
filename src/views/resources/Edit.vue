<template>
  <gov-width-container>
    <ck-loader v-if="loading" />

    <template v-else>
      <vue-headful :title="`${appName} - Edit Resource: ${resource.name}`" />

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
            <gov-heading size="xl">Resources</gov-heading>

            <gov-heading size="m">Edit resource</gov-heading>

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

            <gov-warning-text>
              Please be aware, by submitting these changes, any pending updates
              may be overwritten.
            </gov-warning-text>

            <gov-button v-if="form.$submitting" disabled type="submit">
              Requesting...
            </gov-button>

            <gov-button v-else @click="onSubmit" type="submit">
              Request update
            </gov-button>

            <ck-submit-error v-if="form.$errors.any()" />
          </gov-grid-column>
        </gov-grid-row>
      </gov-main-wrapper>
    </template>
  </gov-width-container>
</template>

<script>
import http from "@/http";
import Form from "@/classes/Form";
import ResourceForm from "@/views/resources/forms/ResourceForm";

export default {
  name: "EditResource",

  components: { ResourceForm },

  data() {
    return {
      loading: false,
      resource: null,
      form: null
    };
  },

  methods: {
    async fetchResource() {
      this.loading = true;

      const { data: { data: resource } } = await http.get(
        `/resources/${this.$route.params.resource}`
      );

      this.resource = resource;

      this.form = new Form({
        organisation_id: this.resource.organisation_id,
        name: this.resource.name,
        slug: this.resource.slug,
        description: this.resource.description,
        url: this.resource.url,
        license: this.resource.license || "",
        author: this.resource.author || "",
        category_taxonomies: this.resource.category_taxonomies.map(
          taxonomy => taxonomy.id
        ),
        published_at: this.resource.published_at || "",
        last_modified_at: this.resource.last_modified_at || ""
      });

      this.loading = false;
    },

    async onSubmit() {
      await this.form.put(
        `/resources/${this.resource.id}`,
        (config, data) => {
          // Remove any unchanged values.
          if (data.organisation_id === this.resource.organisation_id) {
            delete data.organisation_id;
          }

          if (data.name === this.resource.name) {
            delete data.name;
          }

          if (data.name === this.resource.name) {
            delete data.name;
          }

          if (data.slug === this.resource.slug) {
            delete data.slug;
          }

          if (data.description === this.resource.description) {
            delete data.description;
          }

          if (data.url === this.resource.url) {
            delete data.url;
          }

          if (data.license === (this.resource.license || "")) {
            delete data.license;
          }

          if (data.author === (this.resource.author || "")) {
            delete data.author;
          }

          if (
            JSON.stringify(data.category_taxonomies) === JSON.stringify(
              this.resource.category_taxonomies.map(taxonomy => taxonomy.id)
            )
          ) {
            delete data.category_taxonomies;
          }

          if (data.published_at === (this.resource.published_at || "")) {
            delete data.published_at;
          }

          if (data.last_modified_at === (this.resource.last_modified_at || "")) {
            delete data.last_modified_at;
          }
        }
      );

      this.$router.push({
        name: "resources-updated",
        params: { resource: this.resource.id }
      });
    }
  },

  created() {
    this.fetchResource();
  }
};
</script>
