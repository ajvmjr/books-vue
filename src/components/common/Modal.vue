<template>
  <div class="modal">
    <div class="modal-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.modal {
  align-items: center;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  position: fixed;
  left: 0;
  top: 0;
  height: 100%;
  justify-content: center;
  width: 100%;
}

.modal-content {
  background: #303030;
  /* background: red; */
  height: 30%;
  width: 50%;
}

@media screen and (max-width: 1500px) {
  .modal-content {
    height: 40%;
  }
}
</style>
