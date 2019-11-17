<template>
  <div>
    <vue-headful :title="`${appName} - Admin: Taxonomies`" />

    <gov-tabs :tabs="tabs">
      <router-view />
    </gov-tabs>
  </div>
</template>

<script>
export default {
  name: "ListTaxonomies",
  data() {
    return {
      tabs: [
        { heading: "Topics", to: { name: "admin-index-taxonomies" } }
      ]
    };
  }
};
</script>
