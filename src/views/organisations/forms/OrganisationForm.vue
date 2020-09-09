<template>
  <div>
    <ck-text-input
      :value="name"
      @input="onInput('name', $event)"
      id="name"
      label="Organisation name"
      type="text"
      :error="errors.get('name')"
    />

    <ck-text-input
      :value="slug"
      @input="onInput('slug', $event)"
      id="slug"
      label="Unique slug"
      type="text"
      :error="errors.get('slug')"
    >
      <gov-hint slot="hint" for="slug">
        This will be used to access the organisation page.<br>
        e.g. example.com/organisations/{{ slug }}
      </gov-hint>
    </ck-text-input>

    <ck-wysiwyg-input
      :value="description"
      @input="onInput('description', $event)"
      id="description"
      label="Please provide a one-line summary of organisation"
      hint="This should be a short line or two that summarises who the organisation is and will appear below the Organisation name on it's page."
      :error="errors.get('description')"
    />

    <ck-text-input
      :value="url"
      @input="onInput('url', $event)"
      id="url"
      label="Organisation website address"
      type="url"
      :error="errors.get('url')"
    />

    <ck-text-input
      :value="phone"
      @input="onInput('phone', $event)"
      id="phone"
      label="Public phone"
      type="tel"
      :error="errors.get('phone')"
    />

    <ck-text-input
      :value="email"
      @input="onInput('email', $event)"
      id="email"
      label="Public email address"
      type="email"
      :error="errors.get('email')"
    />

    <ck-text-input
      :value="address_line_1"
      @input="onInput('address_line_1', $event)"
      id="address_line_1"
      label="Address Line 1"
      type="text"
      :error="errors.get('address_line_1')"
      optional
    />

    <ck-text-input
      :value="address_line_2"
      @input="onInput('address_line_2', $event)"
      id="address_line_2"
      label="Address Line 2"
      type="text"
      :error="errors.get('address_line_2')"
      optional
    />

    <ck-text-input
      :value="address_line_3"
      @input="onInput('address_line_3', $event)"
      id="address_line_3"
      label="Address Line 3"
      type="text"
      :error="errors.get('address_line_3')"
      optional
    />

    <ck-text-input
      :value="city"
      @input="onInput('city', $event)"
      id="city"
      label="City"
      type="text"
      :error="errors.get('city')"
      optional
    />

    <ck-text-input
      :value="county"
      @input="onInput('county', $event)"
      id="county"
      label="County"
      type="text"
      :error="errors.get('county')"
      optional
    />

    <ck-text-input
      :value="postcode"
      @input="onInput('postcode', $event)"
      id="postcode"
      label="Postcode"
      type="text"
      :error="errors.get('postcode')"
      optional
    />

    <ck-select-input
      :value="country"
      @input="onInput('country', $event)"
      id="country"
      label="Country"
      :options="countries"
      :error="errors.get('country')"
      optional
    />

    <gov-form-group v-if="auth.isGlobalAdmin" :invalid="errors.has('is_hidden')">
      <gov-checkboxes>
        <gov-checkbox
          :value="is_hidden"
          @input="onInput('is_hidden', $event)"
          id="is_hidden"
          name="is_hidden"
          label="Hide from search?"
          optional
        />
        <gov-error-message
          v-if="errors.has('is_hidden')"
          v-text="errors.get('is_hidden')"
          for="is_hidden"
        />
      </gov-checkboxes>
    </gov-form-group>

    <ck-image-input
      @input="onInput('logo_file_id', $event.file_id)"
      id="logo"
      label="Organisation logo"
      accept="image/x-png"
      :existing-url="id ? apiUrl(`/organisations/${id}/logo.png?v=${now}`) : undefined"
      optional
    />

  </div>
</template>

<script>
import CkImageInput from "@/components/Ck/CkImageInput";
import countries from "@/storage/countries";

export default {
  name: "OrganisationForm",
  components: { CkImageInput },
  props: {
    errors: {
      required: true,
      type: Object
    },
    name: {
      required: true,
      type: String
    },
    slug: {
      required: true,
      type: String
    },
    description: {
      required: true,
      type: String
    },
    url: {
      required: true,
      type: String
    },
    phone: {
      required: true,
      type: String
    },
    is_hidden: {
      required: true,
      type: Boolean
    },
    email: {
      required: true,
      type: String
    },
    address_line_1: {
      required: true,
      type: String
    },
    address_line_2: {
      required: true,
      type: String
    },
    address_line_3: {
      required: true,
      type: String
    },
    city: {
      required: true,
      type: String
    },
    county: {
      required: true,
      type: String
    },
    postcode: {
      required: true,
      type: String
    },
    country: {
      required: true,
      type: String
    },
    id: {
      required: false,
      type: String
    }
  },
  data() {
    return {
      countries: [
        { text: "", value: "" },
        ...countries
      ]
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
