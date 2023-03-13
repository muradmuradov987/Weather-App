<template>
  <div class="main">
    <SiteNavigation />
    <router-view />
  </div>
</template>


<script>
import SiteNavigation from "@/components/SiteNavigation.vue";

export default {
  components: {
    SiteNavigation,
  },
};
</script>

<style lang="css" scoped>
.main {
  min-height: 100vh;
  background: #00668a;
  display: flex;
  flex-direction: column;
  /* color: #004e71; */
}
</style>