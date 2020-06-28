<template>
  <gov-width-container>
    <vue-headful :title="`${appName} - Admin`" />

    <gov-back-link :to="{ name: 'dashboard' }">Back to dashboard</gov-back-link>

    <gov-main-wrapper>
      <gov-grid-row>
        <gov-grid-column width="full">

          <gov-heading size="xl">Admin</gov-heading>

          <gov-tabs :tabs="tabs" bar :panel="false">
            <gov-section-break size="m" />
            <router-view />
          </gov-tabs>

        </gov-grid-column>
      </gov-grid-row>
    </gov-main-wrapper>
  </gov-width-container>
</template>

<script>
import Auth from "@/classes/Auth";

export default {
  name: "Admin",
  data() {
    return {
      tabs: [
        { heading: "Audit Logs", to: { name: "admin-index" } },
        { heading: "Notifications", to: { name: "admin-index-notifications" } },
        { heading: "Taxonomies", to: { name: "admin-index-taxonomies" } },
        { heading: "Collections", to: { name: "admin-index-collections" } },
        { heading: "Search Engine", to: { name: "admin-index-search-engine" } },
        { heading: "Stale Services", to: { name: "admin-index-stale-services" }, hide: !Auth.isSuperAdmin }
      ]
    };
  }
};
</script>
