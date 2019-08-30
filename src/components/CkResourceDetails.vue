<template>
  <gov-table>
    <template slot="body">
      <gov-table-row>
        <gov-table-header top scope="row">Resource name</gov-table-header>
        <gov-table-cell>{{ resource.name }}</gov-table-cell>
      </gov-table-row>

      <gov-table-row>
        <gov-table-header top scope="row">Slug</gov-table-header>
        <gov-table-cell>{{ resource.slug }}</gov-table-cell>
      </gov-table-row>

      <gov-table-row>
        <gov-table-header top scope="row">Description</gov-table-header>
        <gov-table-cell v-html="toHtml(resource.description)" />
      </gov-table-row>

      <gov-table-row>
        <gov-table-header top scope="row">Resource URL</gov-table-header>
        <gov-table-cell break>{{ resource.url }}</gov-table-cell>
      </gov-table-row>

      <gov-table-row>
        <gov-table-header top scope="row">License</gov-table-header>
        <gov-table-cell break>{{ resource.license || '-' }}</gov-table-cell>
      </gov-table-row>

      <gov-table-row>
        <gov-table-header top scope="row">Author</gov-table-header>
        <gov-table-cell break>{{ resource.author || '-' }}</gov-table-cell>
      </gov-table-row>

      <gov-table-row>
        <gov-table-header top scope="row">Published date</gov-table-header>
        <gov-table-cell break>
          {{ resource.published_at ? formatDate(resource.published_at) : '-' }}
        </gov-table-cell>
      </gov-table-row>

      <gov-table-row>
        <gov-table-header top scope="row">Last modified date</gov-table-header>
        <gov-table-cell break>
          {{ resource.last_modified_at ? formatDate(resource.last_modified_at): '-' }}
        </gov-table-cell>
      </gov-table-row>
    </template>
  </gov-table>
</template>

<script>
export default {
  name: "CkResourceDetails",

  props: {
    resource: {
      type: Object,
      required: true
    }
  }
};
</script>
