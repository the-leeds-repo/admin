<template>
  <div>
    <gov-heading size="l">Add location</gov-heading>
    <gov-grid-row>
      <gov-grid-column width="one-half">

        <gov-body>Location details.</gov-body>

        <gov-section-break size="l" />

        <ck-service-location-form
          :errors="errors"
          :location-errors="errors"
          location_type="existing"
          name=""
          :regular_opening_hours="[]"
          :holiday_opening_hours="[]"
          address_line_1=""
          address_line_2=""
          address_line_3=""
          city=""
          county=""
          postcode=""
          country=""
          :has_induction_loop="false"
          :has_wheelchair_access="false"
        />

        <slot />

      </gov-grid-column>
    </gov-grid-row>
  </div>
</template>

<script>
import CkServiceLocationForm from "@/views/service-locations/forms/ServiceLocationForm";

export default {
  name: "LocationTab",
  components: {
    CkServiceLocationForm
  },
  props: {
    errors: {
      required: true
    }
  }
};
</script>
