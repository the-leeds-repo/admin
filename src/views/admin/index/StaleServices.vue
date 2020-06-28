<template>
  <div>
    <vue-headful :title="`${appName} - Admin: Stale Services`" />

    <gov-heading size="l">Stale Services</gov-heading>

    <form @submit.prevent="onSubmit">
      <ck-date-input
        id="last_modified_at"
        v-model="form.last_modified_at"
        :error="form.$errors.get('last_modified_at')"
        @input="form.$errors.clear('last_modified_at')"
        label="Last updated"
      />

      <gov-button
        type="submit"
        :disabled="form.$submitting"
      >
        Disable
      </gov-button>
    </form>
  </div>
</template>

<script>
import Form from "@/classes/Form";
import CkDateInput from "@/views/services/inputs/DateInput";

export default {
  name: "StaleServices",
  components: {
    CkDateInput
  },
  data() {
    return {
      form: new Form({
        last_modified_at: ''
      })
    };
  },
  methods: {
    async onSubmit() {
      if (!window.confirm('Are you sure you want to disable stale services?')) {
        return;
      }

      await this.form.put('/services/disable-stale');

      window.alert('Stale services successfully disabled');
    }
  }
};
</script>
