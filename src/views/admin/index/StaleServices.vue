<template>
  <div>
    <vue-headful :title="`${appName} - Admin: Stale Services`" />

    <gov-heading size="l">Stale Services</gov-heading>

    <gov-body>Lorem</gov-body>
  </div>
</template>

<script>
import Form from "@/classes/Form";

export default {
  name: "StaleServices",
  data() {
    return {
      form: new Form()
    };
  },
  methods: {
    //
  }
};
</script>
