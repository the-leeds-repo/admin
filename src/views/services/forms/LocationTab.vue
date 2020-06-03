<template>
  <div>
    <gov-heading size="l">Add location</gov-heading>
    <gov-grid-row>
      <gov-grid-column width="one-half">

        <gov-body>Location details.</gov-body>

        <gov-section-break size="l" />

        <ck-service-location-form
          :errors="service_location_errors"
          :location-errors="location_errors"
          :location_type.sync="locationType"
          :location_id="location_id"
          :name="name"
          :address_line_1="address_line_1"
          :address_line_2="address_line_2"
          :address_line_3="address_line_3"
          :city="city"
          :county="county"
          :postcode="postcode"
          :country="country"
          :has_induction_loop="has_induction_loop"
          :has_wheelchair_access="has_wheelchair_access"
          :regular_opening_hours="regular_opening_hours"
          :holiday_opening_hours="holiday_opening_hours"
          @update:location_id="onInput({ field: 'location_id', value: $event })"
          @update:name="onInput({ field: 'name', value: $event })"
          @update:address_line_1="onInput({ field: 'address_line_1', value: $event })"
          @update:address_line_2="onInput({ field: 'address_line_2', value: $event })"
          @update:address_line_3="onInput({ field: 'address_line_3', value: $event })"
          @update:city="onInput({ field: 'city', value: $event })"
          @update:county="onInput({ field: 'county', value: $event })"
          @update:postcode="onInput({ field: 'postcode', value: $event })"
          @update:country="onInput({ field: 'country', value: $event })"
          @update:has_induction_loop="onInput({ field: 'has_induction_loop', value: $event })"
          @update:has_wheelchair_access="onInput({ field: 'has_wheelchair_access', value: $event })"
          @update:regular_opening_hours="onInput({ field: 'regular_opening_hours', value: $event })"
          @update:holiday_opening_hours="onInput({ field: 'holiday_opening_hours', value: $event })"
          @update:image_file_id="onInput({ field: 'image_file_id', value: $event })"
          @clear="onLocationClear"
          @clear-location="onServiceLocationClear"
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
    service_location_errors: {
      required: true
    },
    location_errors: {
      required: true
    },
    location_id: {
      required: true
    },
    name: {
      required: true
    },
    address_line_1: {
      required: true
    },
    address_line_2: {
      required: true
    },
    address_line_3: {
      required: true
    },
    city: {
      required: true
    },
    county: {
      required: true
    },
    postcode: {
      required: true
    },
    country: {
      required: true
    },
    has_induction_loop: {
      required: true
    },
    has_wheelchair_access: {
      required: true
    },
    regular_opening_hours: {
      required: true
    },
    holiday_opening_hours: {
      required: true
    },
  },
  data() {
    return {
      locationType: null
    }
  },
  methods: {
    onInput({ field, value }) {
      this.$emit(`update:${field}`, value);
    },
    onLocationClear(field) {
      this.$emit('location-clear', field);
    },
    onServiceLocationClear(field) {
      this.$emit('service-location-clear', field);
    }
  }
};
</script>
