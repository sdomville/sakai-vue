<template>
  <div v-if="isVisible" class="modal-backdrop">
    <div class="col-md-6 modal-content">
      <slot name="content" />
      <!-- <div class="content">
        <div class="container-fluid">
          <div class="row">
            <div class="col-md-8">

            </div>
          </div>
        </div>
      </div> -->
      <!-- <button class="btn btn-info float-right" @click="closeModal">Cancel</button> -->

    </div>
  </div>
</template>

<script>
export default {
  name: 'ModalComponent',
  props: {
    isVisible: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    closeModal() {
      this.$emit('close');
    },
  },
};
</script>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  max-height: 85vh;
  overflow-y: auto;
}
</style>
