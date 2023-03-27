<template>
  <Transition name="fade">
    <div
      v-if="this.$store.state.activeModal"
      class="modal__container bg-black bg-opacity-50 position-absolute w-100 d-flex flex-column align-items-center"
    >
      <div class="modal__body rounded-2">
        <div class="mb-4">
          <h3>Info:</h3>
          <p>
            Lorem ipsum dolor, sit amet consectetur adipisicing elit. Qui
            temporibus ut nostrum harum alias. Debitis excepturi voluptas sequi
            exercitationem, doloribus odio repellat et illo eum delectus iure
            nam sunt alias! Praesentium, excepturi amet voluptates eaque saepe
            nostrum veniam perspiciatis eos impedit iusto doloremque soluta
            fugit quas harum iste? Debitis, ducimus.
          </p>
        </div>
        <button
          @click="closeModal()"
          class="text-white py-2 px-3 close rounded-2 border-0"
        >
          Close
        </button>
      </div>
    </div>
  </Transition>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    closeModal() {
      this.$store.state.activeModal = false;
    },
  },
};
</script>

<style lang="css" scoped>
.modal__container {
  height: 100vh;
  top: 0;
  left: 0;
}
.modal__body {
  transition: 0.5s ease;
  margin-top: 100px;
  width: 50%;
  z-index: 10;
  background: white;
  padding: 30px;
}
.close {
  background: #00668a;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

@media (max-width: 768px) {
  .modal__body {
  margin-top: 50px;
  width: 90%;
  padding: 20px;
}
}
</style>
