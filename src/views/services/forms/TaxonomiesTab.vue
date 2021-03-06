<template>
  <div>
    <gov-heading size="l">Taxonomies (Topics)</gov-heading>
    <gov-grid-row>
      <gov-grid-column width="one-half">
        <gov-body>
          These are a list of ‘tags’ that are applied to a {{ type }}. These tags
          help the {{ type }} be found in topics and keyword searches.
        </gov-body>
        <gov-body>
          On creation of a new {{ type }}, the admin team will select the tags that
          they feel represent the {{ type }} offer.
        </gov-body>

        <gov-section-break size="l" />

        <gov-body>Some of the topics you may find, may have a parent that states for authorised use only after it.</gov-body>
        <gov-body>For example: Accreditation (authorised use only)</gov-body>
        <gov-body>This indicates that any child topics underneath this, should only be used if you are aware of their meaning and can confirm any processes behind them have been followed.</gov-body>
        <gov-body>For example, a topic may say 'Clinically accredited', which indicates a clinician has followed a specific formal process to accredit the service or resource.</gov-body>

        <category-taxonomy-input
          :value="category_taxonomies"
          @input="$emit('update:category_taxonomies', $event)"
          @clear="$emit('clear', 'category_taxonomies')"
          :error="errors.get('category_taxonomies')"
          :invalid="errors.has('category_taxonomies')"
        />

        <slot />

      </gov-grid-column>
    </gov-grid-row>
  </div>
</template>
<script>
import CategoryTaxonomyInput from "@/views/services/inputs/CategoryTaxonomyInput";

export default {
  name: "TaxonomiesTab",
  components: { CategoryTaxonomyInput },
  props: {
    errors: {
      required: true
    },
    type: {
      required: true,
      type: String
    },
    category_taxonomies: {
      required: true,
      type: Array
    }
  },
  computed: {
    contactAdminTeamEmail() {
      const to = this.contactEmail;
      const subject = `Incorrect taxonomies applied to ${this.type}`;
      const body = `${this.$options.filters.ucfirst(
        this.type
      )} Name: XXX\n\nI believe that the tags applied to the above ${
        this.type
      } are incorrect. The following changes should be made:`;

      return `mailto:${to}?subject=${encodeURIComponent(
        subject
      )}&body=${encodeURIComponent(body)}`;
    }
  }
};
</script>
