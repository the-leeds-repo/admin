<template>
  <gov-table>
    <template slot="body">
      <gov-table-row>
        <gov-table-header top scope="row">Organisation name</gov-table-header>
        <gov-table-cell>{{ organisation.name }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header top scope="row">Description</gov-table-header>
        <gov-table-cell v-html="toHtml(organisation.description)" />
      </gov-table-row>
      <gov-table-row>
        <gov-table-header top scope="row">Website Address</gov-table-header>
        <gov-table-cell break>{{ organisation.url }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header top scope="row">Phone number</gov-table-header>
        <gov-table-cell>{{ organisation.phone }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header top scope="row">Email</gov-table-header>
        <gov-table-cell>{{ organisation.email }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header scope="row" top>Address line 1</gov-table-header>
        <gov-table-cell>{{ organisation.address_line_1 || "-" }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header scope="row" top>Address line 2</gov-table-header>
        <gov-table-cell>{{ organisation.address_line_2 || "-" }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header scope="row" top>Address line 3</gov-table-header>
        <gov-table-cell>{{ organisation.address_line_3 || "-" }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header scope="row" top>City</gov-table-header>
        <gov-table-cell>{{ organisation.city || "-" }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header scope="row" top>County</gov-table-header>
        <gov-table-cell>{{ organisation.county || "-" }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header scope="row" top>Postcode</gov-table-header>
        <gov-table-cell>{{ organisation.postcode || "-" }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header scope="row" top>Country</gov-table-header>
        <gov-table-cell>{{ organisation.country || "-" }}</gov-table-cell>
      </gov-table-row>
      <gov-table-row>
        <gov-table-header top scope="row">Hide from search?</gov-table-header>
        <gov-table-cell>{{ organisation.is_hidden ? 'Hidden' : 'Visible' }}</gov-table-cell>
      </gov-table-row>
      <template v-if="auth.isGlobalAdmin">
        <gov-table-row>
          <gov-table-header top scope="row">Sync with CiviCRM?</gov-table-header>
          <gov-table-cell>{{ organisation.civi_sync_enabled ? 'Yes' : 'No' }}</gov-table-cell>
        </gov-table-row>
        <gov-table-row>
          <gov-table-header top scope="row">CiviCRM ID</gov-table-header>
          <gov-table-cell>{{ organisation.civi_id || '-' }}</gov-table-cell>
        </gov-table-row>
      </template>
      <gov-table-row>
        <gov-table-header top scope="row">Logo</gov-table-header>
        <gov-table-cell>
          <img :src="apiUrl(`/organisations/${organisation.id}/logo.png?v=${organisation.updated_at}`)" alt="Organisation logo" class="ck-logo">
        </gov-table-cell>
      </gov-table-row>
    </template>
  </gov-table>
</template>

<script>
export default {
  name: "CkOrganisationDetails",
  props: {
    organisation: {
      type: Object,
      required: true
    }
  }
};
</script>
