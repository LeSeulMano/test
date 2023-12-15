<template>
  <div class="overlay" ref="overlay" v-if="errorModalVisible" @click="closeErrorModal"></div>
  <div v-if="errorModalVisible" class="error-modal" ref="errorModal">
    <div class="error-header">
      <h4>Error !</h4>
      <div class="close-modal" @click="closeErrorModal">
        <IonIcon name="close-outline"></IonIcon>
      </div>
    </div>
    <div class="error-content">
      <p>
        <span>{{message}}</span>
      </p>
      <h6>Si l'erreur persiste, contactez l'administrateur</h6>
    </div>
  </div>
</template>

<script>
import {IonIcon} from '@ionic/vue';
import anime from "animejs";
export default {

  components: {
    IonIcon
  },
  data() {
    return {
      errorMessage: '',
    };
  },
  props: {
    message: String,
    errorModalVisible: Boolean
  },
  methods: {
    closeErrorModal() {
      anime({
        targets: this.$refs.overlay,
        opacity: 0,
      });
      anime({
        targets: this.$refs.errorModal,
        translateX: ["0", "500%"],
        duration: 500,
        complete: () => {
          this.$emit('update:errorModalVisible', false);
        },
      });
    },
  }

}

</script>

<style lang="scss" scoped>
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

.error-modal {
  position: fixed;
  width: 40rem;
  background-color: $primary-white;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  border-radius: 5%;
  overflow: hidden;
  z-index: 1000;

  .error-header {
    height: 20%;
    width: 100%;
    top: 0;
    left: 0;
    background-color: $primary-red;
    display: flex;
    justify-content: center;
    align-items: center;

    h4 {
      font-size: $h4-text;
      color: $secondary-white;
    }

    .close-modal {
      position: absolute;
      top: 10px;
      right: 10px;
      background-color: white;
      border-radius: 50%;
      padding: 5px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;

      ion-icon {
        font-size: 20px; // ajustez la taille de l'icône selon vos besoins
        color: $primary-red; // ajustez la couleur selon vos besoins
      }
    }
  }

  .error-content {
    max-height: 20rem;
    margin: .8rem 0 1rem 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;

    p {
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-line-clamp: 5;
      -webkit-box-orient: vertical;
    }

    h6 {
      font-size: $p-text;
    }

  }
}
</style>