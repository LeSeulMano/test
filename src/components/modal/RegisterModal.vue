<template>
  <div v-if="showModal" class="overlay" ref="overlay"></div>
  <div v-if="showModal" class="modal">
    <div class="modal-content">
      <div class="icon-container">
        <IonIcon name="checkmark-circle-outline"></IonIcon>
      </div>
      <h4>Un email de confirmation a été envoyé !</h4>
    </div>
  </div>
</template>

<script>
import {IonIcon} from "@ionic/vue";

export default {
  components: {IonIcon},
  props: {
    showModal: Boolean,
  }
}
</script>

<style lang="scss">
@import "../../utils/computer/components.scss";

.overlay {
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(8px);
  z-index: 999;
  transition: opacity 0.5s;
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal {
  position: fixed;
  background-color: $primary-white;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  border-radius: 5%;
  overflow: hidden;
  z-index: 1000;

  .modal-content {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    transform-origin: center center;
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;

    .icon-container {
      display: flex;
      justify-content: center;
      align-items: center;
      border: 2px solid #4caf50;
      border-radius: 50%;
      width: 60px;
      height: 60px;
      font-size: 2rem;
      color: #4caf50;
    }

  }
}
</style>