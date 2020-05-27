<template>
  <ck-loader v-if="loading" />

  <!-- Level: 1 -->
  <gov-form-group v-else>
    <vue-treeselect
      :value="value"
      @input="onInput"
      :options="taxonomies"
      value-consists-of="ALL_WITH_INDETERMINATE"
      :normalizer="normalizer"
      :multiple="true"
      placeholder="Select associated taxonomies..."
      sort-value-by="INDEX"
    />

    <gov-error-message
      v-if="error"
      v-text="error"
      for="category_taxonomies"
    />
  </gov-form-group>
  <!-- /Level: 1 -->
</template>

<script>
import http from "@/http";
import VueTreeselect from '@riophae/vue-treeselect'
import '@riophae/vue-treeselect/dist/vue-treeselect.css'

export default {
  name: "CategoryTaxonomyInput",
  components: {
    VueTreeselect
  },
  props: {
    value: {
      required: true,
      type: Array
    },
    error: {
      required: true
    },
    disabled: {
      required: false,
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      taxonomies: [],
      loading: false
    };
  },
  methods: {
    async fetchTaxonomies() {
      this.loading = true;
      const { data: { data: taxonomies } } = await http.get("/taxonomies/categories");
      this.taxonomies = taxonomies.map(child => this.removeEmptyChildren(child));
      this.loading = false;
    },
    removeEmptyChildren(taxonomy) {
      if (taxonomy.children.length === 0) {
        delete taxonomy.children;
      } else {
        taxonomy.children.map(child => this.removeEmptyChildren(child))
      }

      return taxonomy
    },
    normalizer({ id, name, children }) {
      return {
        id,
        label: name,
        children
      }
    },
    onInput(taxonomyIds) {
      this.$emit('input', taxonomyIds);
    }
  },
  created() {
    this.fetchTaxonomies();
  }
};
</script>
