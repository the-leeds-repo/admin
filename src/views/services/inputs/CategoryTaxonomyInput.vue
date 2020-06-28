<template>
  <ck-loader v-if="loading" />

  <gov-form-group v-else>
    <vue-treeselect
      :value="value"
      @input="onInput"
      :options="taxonomies"
      :value-consists-of="valueConsistsOf"
      :normalizer="normalizer"
      :multiple="true"
      placeholder="Select associated taxonomies..."
      sort-value-by="INDEX"
      :disabled="disabled"
    />

    <gov-error-message
      v-if="error"
      v-text="error"
      for="category_taxonomies"
    />
  </gov-form-group>
</template>

<script>
import http from "@/http";
import VueTreeselect from "@riophae/vue-treeselect";

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
    },
    valueConsistsOf: {
      required: false,
      type: String,
      default: "ALL_WITH_INDETERMINATE"
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
      this.$emit('clear')
    }
  },
  created() {
    this.fetchTaxonomies();
  }
};
</script>

<style lang="scss">
@import url("~@riophae/vue-treeselect/dist/vue-treeselect.css");
.vue-treeselect {
  font-family: nta, Arial, sans-serif !important,
}
</style>
