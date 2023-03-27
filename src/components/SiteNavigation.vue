<template>
  <header class="sticky-top">
    <div class="container">
      <nav class="d-flex align-items-center justify-content-between">
        <router-link :to="{ name: 'home' }" class="text-decoration-none">
          <div class="d-flex align-items-center">
            <i class="fa-solid fa-sun text-white fs-4 fw-bold me-4"></i>
            <p class="m-0 text-white fs-4">The Local Weather</p>
          </div>
        </router-link>

        <div class="d-flex align-items-center justify-content-center">
          <i
            @click="openModal()"
            class="fa-solid fa-circle-info fs-4 fw-bold me-3 info"
          ></i>
        </div>
          <BaseModal/>
      </nav>
    </div>
  </header>
</template>

<script>
import BaseModal from "@/components/BaseModal.vue";
export default {
  components: {
    BaseModal,
  },
  data() {
    return {};
  },
  methods: {
    openModal() {
      this.$store.state.activeModal = true;
    },
    closeModal() {
      this.$store.state.activeModal = false;
    },
  },
};
</script>

<style lang="css" scoped>
header {
  box-shadow: -2px 9px 14px -3px rgba(0, 0, 0, 0.75);
  background: #00668a;
}
nav {
  height: 50px;
}
.info {
  color: white;
  transition: 0.3s ease;
  cursor: pointer;
}
.info:hover {
  color: #004e71;
}




</style>
