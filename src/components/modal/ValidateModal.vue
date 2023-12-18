<template>
  <div class="overlay" ref="overlay" v-if="validateModalVisible" @click="closeValidateModal"></div>
  <div v-if="validateModalVisible" class="validate-modal" ref="validatemodal">
    <div class="validate-header">
      <div class="close-modal" @click="closeValidateModal">
        <IonIcon name="close-outline"></IonIcon>
      </div>
    </div>
    <div class="sent">
      <h3>Vous êtes sur le point de supprimer votre compte <br></h3>
      <h3>Etes vous sur de vouloir faire cela ?</h3>
      <div class="btn btn-secondary btn-icon-forward pointer">
        <router-link class="router-link" to="/" @click="removeAccount">
          <div>Oui</div>
        </router-link>
      </div>
      <div class="btn btn-primary btn-icon-forward pointer">
        <div class="router-link" @click="closeValidateModal">
          <div>Non</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import {IonIcon} from '@ionic/vue';
import anime from "animejs";
import {IonIcon} from "@ionic/vue";
import axios from "axios";
export default {

  components: {
    IonIcon,
  },
  props: {
    validateModalVisible: Boolean
  },
  methods: {
    closeValidateModal() {
      anime({
        targets: this.$refs.overlay,
        opacity: 0,
      });
      anime({
        targets: this.$refs.validatemodal,
        translateX: ["0", "500%"],
        duration: 500,
        complete: () => {
          this.$emit('update:validateModalVisible', false);
        },
      });
    },
    removeAccount(){
      axios.get("https://delmoo.fr:5000/remove", {
        withCredentials: true,
      })
    }
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

.validate-modal {
  position: fixed;
  width: 40rem;
  background-color: $primary-white;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  border-radius: 15px;
  overflow: hidden;
  z-index: 1000;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  .sent{
    padding: 2rem;
    .btn{
      margin-left: 25%;
    }
  }

  .validate-header {
    height: 20%;
    width: 100%;
    top: 0;
    left: 0;
    background-color: $primary-red;
    display: flex;
    justify-content: center;
    align-items: center;

    .close-modal {
      position: absolute;
      top: 10px;
      right: 10px;
      background-color: $primary-red;
      border-radius: 50%;
      padding: 5px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;

      ion-icon {
        font-size: 20px;
        color: $primary-white;
      }
    }
  }

}
</style>