<template>
  <label class="govuk-label">
    <slot /><template v-if="!optional">*</template>
  </label>
</template>

<script>
export default {
  name: "GovLabel",

  props: {
    optional: {
      type: Boolean,
      required: false,
      default: false
    }
  }
};
</script>
