<template>
  <div class="govuk-date-input">
    <div class="govuk-date-input__item">
      <div class="govuk-form-group">
        <label
          class="govuk-label govuk-date-input__label"
          :for="`${id}-passport-issued-day`"
        >
          Day
        </label>

        <input
          class="govuk-input govuk-date-input__input govuk-input--width-2"
          :id="`${id}-passport-issued-day`"
          name="passport-issued-day"
          type="number"
          pattern="[0-9]*"
          :value="day"
          @input="onInput({ day: $event.target.value })"
          :disabled="disabled"
        >
      </div>
    </div>

    <div class="govuk-date-input__item">
      <div class="govuk-form-group">
        <label
          class="govuk-label govuk-date-input__label"
          :for="`${id}-passport-issued-month`"
        >
          Month
        </label>

        <input
          class="govuk-input govuk-date-input__input govuk-input--width-2"
          :id="`${id}-passport-issued-month`"
          name="passport-issued-month"
          type="number"
          pattern="[0-9]*"
          :value="month"
          @input="onInput({ month: $event.target.value })"
          :disabled="disabled"
        >
      </div>
    </div>

    <div class="govuk-date-input__item">
      <div class="govuk-form-group">
        <label
          class="govuk-label govuk-date-input__label"
          :for="`${id}-passport-issued-year`"
        >
          Year
        </label>

        <input
          class="govuk-input govuk-date-input__input govuk-input--width-4"
          :id="`${id}-passport-issued-year`"
          name="passport-issued-year"
          type="number"
          pattern="[0-9]*"
          :value="year"
          @input="onInput({ year: $event.target.value })"
          :disabled="disabled"
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      required: true
    },

    disabled: {
      type: Boolean,
      required: false,
      default: false
    }
  },

  data() {
    return {
      id: null
    };
  },

  computed: {
    parts() {
      return this.value.split("-");
    },

    day() {
      return this.parts.length === 1 ? "" : this.parts[2];
    },

    month() {
      return this.parts.length === 1 ? "" : this.parts[1];
    },

    year() {
      return this.parts.length === 1 ? "" : this.parts[0];
    }
  },

  methods: {
    onInput({ day = this.day, month = this.month, year = this.year }) {
      if (day === "" && month === "" && year === "") {
        this.$emit("input", "");
        return;
      }

      this.$emit("input", `${year}-${month}-${day}`);
    }
  },

  mounted() {
    this.id = this._uid;
  }
};
</script>
