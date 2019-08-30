<template>
  <div>
    <ck-loader v-if="loadingOrganisations" />
    <ck-select-input
      v-else
      :value="organisation_id"
      @input="onInput('organisation_id', $event)"
      id="organisation_id"
      label="Organisation"
      hint="Which organisation hosts this resource?"
      :options="organisations"
      :error="errors.get('organisation_id')"
    />

    <ck-text-input
      :value="name"
      @input="onNameInput($event)"
      id="name"
      label="Name of resource"
      type="text"
      :error="errors.get('name')"
    />

    <ck-text-input
      :value="slug"
      @input="onInput('slug', $event)"
      id="slug"
      label="Unique slug"
      type="text"
      :error="errors.get('slug')"
    >
      <gov-hint slot="hint" for="slug">
        This will be used to access the resource page.<br>
        e.g. example.com/resources/{{ slug }}
      </gov-hint>
    </ck-text-input>

    <ck-text-input
      :value="url"
      @input="onInput('url', $event)"
      id="url"
      label="What is the web address of your resource?"
      hint="This must start with ‘http://’ or ‘https://’."
      type="url"
      :error="errors.get('url')"
    />

    <ck-wysiwyg-input
      :value="description"
      @input="onInput('description', $event)"
      id="description"
      label="Description"
      hint="This is the largest body of text on your page. Fill it with everything else someone should know about your resource. Use headers, bullets and formatting for the maximum effect."
      :error="errors.get('description')"
      large
      :maxlength="1600"
    />

    <ck-text-input
      :value="license"
      @input="onInput('license', $event)"
      id="license"
      label="License"
      hint="If this resource has a license, then please specify it here."
      type="text"
      :error="errors.get('license')"
    />

    <ck-text-input
      :value="author"
      @input="onInput('author', $event)"
      id="author"
      label="Author"
      hint="This will usually be the person or company who provided the resource."
      type="text"
      :error="errors.get('author')"
    />

    <ck-date-input
      :value="published_at"
      @input="onInput('published_at', $event)"
      id="published_at"
      label="Published date"
      hint="This is the date which the resource was first published."
      :error="errors.get('published_at')"
    />

    <ck-date-input
      :value="last_modified_at"
      @input="onInput('last_modified_at', $event)"
      id="last_modified_at"
      label="Last modified date"
      hint="This is the date which the resource was last modified (since being published)."
      :error="errors.get('last_modified_at')"
    />
  </div>
</template>

<script>
import CkDateInput from "@/components/Ck/CkDateInput.vue";

export default {
  name: "CreateResourceForm",

  components: {
    CkDateInput
  },

  props: {
    errors: {
      required: true,
      type: Object
    },

    organisation_id: {
      required: true,
      type: String
    },

    name: {
      required: true,
      type: String
    },

    slug: {
      required: true,
      type: String
    },

    description: {
      required: true,
      type: String
    },

    url: {
      required: true,
      type: String
    },

    license: {
      required: true,
      type: String
    },

    author: {
      required: true,
      type: String
    },

    category_taxonomies: {
      required: true,
      type: Array
    },

    published_at: {
      required: true,
      type: String
    },

    last_modified_at: {
      required: true,
      type: String
    }
  },

  data() {
    return {
      loadingOrganisations: false,
      organisations: []
    };
  },

  methods: {
    onInput(field, value) {
      this.$emit(`update:${field}`, value);
      this.$emit("clear", field);
    },

    onNameInput(name) {
      this.$emit("update:name", name);
      this.$emit("clear", "name");

      if (this.auth.isGlobalAdmin || this.isNew) {
        this.$emit("update:slug", this.slugify(name));
        this.$emit("clear", "slug");
      }
    },

    async fetchOrganisations() {
      this.loadingOrganisations = true;

      let fetchedOrganisations = await this.fetchAll("/organisations", {
        "filter[has_permission]": true
      });

      fetchedOrganisations = fetchedOrganisations.map(organisation => {
        return { text: organisation.name, value: organisation.id };
      });

      this.organisations = [...this.organisations, ...fetchedOrganisations];

      this.loadingOrganisations = false;
    }
  },

  created() {
    this.fetchOrganisations();
  }
};
</script>
