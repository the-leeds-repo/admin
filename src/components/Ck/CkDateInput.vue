<template>
  <gov-form-group :invalid="error !== null">
    <gov-label :for="id" class="govuk-!-font-weight-bold" :optional="optional">
      <slot name="label">{{ label }}</slot>
    </gov-label>

    <slot name="hint">
      <gov-hint v-if="hint" :for="id" v-text="hint" />
    </slot>

    <gov-date-input
      :value="value"
      @input="$emit('input', $event)"
      :disabled="disabled"
    />

    <slot name="after-input" />

    <gov-error-message
      v-if="error !== null"
      v-text="error"
      :for="id"
    />
  </gov-form-group>
</template>

<script>
import GovDateInput from "@/components/Gov/GovDateInput.vue";

export default {
  components: {
    GovDateInput
  },

  props: {
    value: {
      required: true,
      type: String
    },

    label: {
      required: true,
      type: String
    },

    hint: {
      required: false,
      type: String
    },

    error: {
      required: true
    },

    id: {
      required: true,
      type: String
    },

    disabled: {
      required: false,
      type: Boolean,
      default: false
    },

    optional: {
      type: Boolean,
      required: false,
      default: false
    }
  }
};
</script>
