<template>
  <div>
    <ck-text-input
      :value="code"
      @input="onInput('code', $event)"
      id="code"
      label="Code"
      type="text"
      :error="errors.get('code')"
    />

    <ck-text-input
      :value="name"
      @input="onInput('name', $event)"
      id="name"
      label="Name"
      type="text"
      :error="errors.get('name')"
    />

    <gov-label class="govuk-!-font-weight-bold">Taxonomies</gov-label>

    <category-taxonomy-input
      :invalid="errors.has('category_taxonomies')"
      :value="category_taxonomies"
      @input="$emit('update:category_taxonomies', $event)"
      :error="errors.get('category_taxonomies')"
      @clear="$emit('clear', 'category_taxonomies')"
    />
  </div>
</template>

<script>
import CategoryTaxonomyInput from "@/views/services/inputs/CategoryTaxonomyInput";

export default {
  name: "CollectionForm",
  components: {
    CategoryTaxonomyInput
  },

  props: {
    errors: {
      required: true,
      type: Object
    },

    code: {
      required: true
    },

    name: {
      required: true
    },

    order: {
      required: true
    },

    category_taxonomies: {
      required: true
    }
  },

  methods: {
    onInput(field, value) {
      this.$emit(`update:${field}`, value);
      this.$emit("clear", field);
    }
  }
};
</script>
