<template>
  <gov-width-container>
    <vue-headful :title="`${appName} - Add Service`" />

    <gov-back-link :to="{ name: 'services-index' }">
      Back to services
    </gov-back-link>

    <gov-main-wrapper>
      <gov-grid-row>
        <gov-grid-column width="one-half">
          <gov-heading size="xl">Check for duplicated services</gov-heading>

          <gov-body>Before adding a service, you must perform a search on the directory to ensure that the service doesn't already exist.</gov-body>

          <gov-form-group>
            <gov-label for="confirmed" optional>Have you performed a duplicate search?</gov-label>
            <gov-checkboxes>
              <gov-checkbox
                v-model="confirmed"
                id="confirmed"
                name="confirmed"
                label="I confirm I have performed a duplicate search"
              />
            </gov-checkboxes>
          </gov-form-group>

          <gov-button :to="{name: 'services-index'}">
            Back to services
          </gov-button><!--
          -->&nbsp;<!--
          --><gov-button :disabled="!confirmed" :to="confirmed ? { name: 'services-pre-create-2' } : undefined">
            Continue
          </gov-button>
        </gov-grid-column>
      </gov-grid-row>
    </gov-main-wrapper>
  </gov-width-container>
</template>

<script>
export default {
  data() {
    return {
      confirmed: false
    }
  }
}
</script>
