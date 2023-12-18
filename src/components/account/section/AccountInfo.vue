<template>
  <div class="user-group" v-if="isShow">
    <h2>{{ information.username }} : </h2>
    <h2 v-if="information.is_checked == 0" class="warning">Attention vous n'avez pas vérifié votre adresse mail uphf !</h2>
    <div class="user-information">
      <h3>Adresse mail: </h3>
      <p>{{ information.email }}</p>
      <div class="btn pointer btn-primary btn-icon" @click="emitChangePassword(2)">
        <div>
          <IonIcon name="pencil-outline"></IonIcon>
        </div>
      </div>
    </div>
    <div class="user-information">
      <h3>Nom d'utillisateur: </h3>
      <p>{{ information.username }}</p>
      <div class="btn pointer btn-primary btn-icon" @click="emitChangePassword(1)">
        <div>
          <IonIcon name="pencil-outline"></IonIcon>
        </div>
      </div>
    </div>
    <div class="btn-action">
      <div class="btn pointer btn-primary btn-icon" @click="emitChangePassword(0)">
        <div>
          <div>Changer de mot de passe</div>
        </div>
      </div>
      <div class="btn pointer btn-secondary btn-icon"  @click="showValidateModal">
        <div>
          <div>Supprimer le compte</div>
        </div>
      </div>

    </div>

  </div >
  <ErrorModal :errorModalVisible="errorModalVisible" :message="errorMessage"
              @update:errorModalVisible="updateErrorModal"></ErrorModal>
  <ValidateModal :validateModalVisible="validateModalVisible" @update:validateModalVisible="updateValidateModal"></ValidateModal>
</template>

<script>
import {IonIcon} from "@ionic/vue";
import ErrorModal from "@/components/modal/ErrorModal.vue";
import ValidateModal from "@/components/modal/ValidateModal.vue";
import axios from "axios";
import anime from "animejs";

export default {
  components: {
    IonIcon,
    ErrorModal,
    ValidateModal
  },
  emits: ['change-tab'],
  props: {
    isShow: Boolean
  },
  data() {
    return {
      information: {},
      errorModalVisible: false,
      errorMessage: "",
      validateModalVisible: false
    }
  },
  mounted() {
    const screenHeight = window.innerHeight;
    document.querySelector('#sect11').style.height = screenHeight + "px";

    axios.get("https://delmoo.fr:5000/current-user", {
      withCredentials: true,
      validateStatus: function (status) {
        return status === 201 || status === 500;
      }
    }).then((res) => {
      if (res.status == 201) {
        this.information = res.data.json[0];
      } else {
        this.showErrorModal(res.data.message);
        return;
      }
    })
  },
  methods: {
    emitChangePassword(changing) {
      switch (changing){
        case 0 :
          this.$emit('change-tab', 'ChangePassword');
          break;
        case 1:
          this.$emit('change-tab', 'ChangeUsername');
          break;
        case 2:
          this.$emit('change-tab', 'ChangeEmail');
          break;
      }
    },
    showErrorModal(message) {
      this.errorMessage = message;
      this.errorModalVisible = true;
      this.$nextTick(() => {
        anime({
          targets: this.$refs.overlay,
          opacity: 1,
          duration: 500,
        });
        anime({
          targets: ".error-modal",
          translateX: ["200%", "0%"],
          easing: 'easeOutElastic(.5, .3)',
          duration: 500
        });
      })
    },
    updateErrorModal(value) {
      this.errorModalVisible = value;
    },
    showValidateModal() {
      this.validateModalVisible = true;
      this.$nextTick(() => {
        anime({
          targets: this.$refs.overlay,
          opacity: 1,
          duration: 500,
        });
        anime({
          targets: ".validate-modal",
          translateX: ["200%", "0%"],
          easing: 'easeOutElastic(.5, .3)',
          duration: 500
        });
      })
    },
    updateValidateModal(value) {
      this.validateModalVisible = value;
    },
  }
}
</script>

<style lang="scss">
@import '../../../utils/computer/components.scss';

.user-group {
  position: absolute;
  left: 50%;
  top: 60%;
  transform: translate(-50%, -50%);
  display: flex;
  flex-direction: column;
  max-height: 28rem;


  // Effet Glass Morphism
  background: rgba($color: $terciary-white, $alpha: .5); // rgba(35,35,35,0.5)
  -webkit-backdrop-filter: blur(10px);
  backdrop-filter: blur(10px);
  border: 1px solid rgba($color: $terciary-white, $alpha: .25); //rgba(35,35,35,0.25)

  // Effet d'ombre
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

  border-radius: $half-round;
  padding: 2.4rem 3rem 2rem 3rem;

  h2 {
    text-align: center;
  }

  .warning {
    color: $primary-red;
  }

  .user-information {
    display: flex;
    justify-content: space-between;
    position: relative;

    p {
      margin-right: 10rem;
      margin-left: 10rem;
    }

    .btn {
      position: absolute;
      right: 0;
    }
  }

  .btn-action {
    margin-top: 2rem;
    margin-left: 30%;
    width: 18rem;
    display: flex;
    flex-direction: column;

    .btn {
      margin-top: .5rem;
    }
  }

  .history{
    margin-top: 1rem;
    position: relative;
    width: 30rem;
    cursor: pointer;
    margin-left: 7rem;
    &:after {
      position: absolute;
      content: '';
      width: 90%;
      top: 100%;
      left: 47%;
      transform: translateX(-50%);
      height: 2px;
      background-color: $primary-red;
      transition: .4s;
    }

    &:hover::after {
      width: 50%;

    }
  }

}

@media only screen and (max-width: 1024px) {
  $h6-text: 1.2rem;
  $h3-text: 3.8rem;
  $h5-text: 2.4rem;
  $h4-text: 2.9rem;
    .user-group{
      top: 50%;
      max-width: 20rem;
      .user-information {
        display: flex;
        justify-content: space-between;
        position: relative;

        p {
          margin-right: 5rem;
          margin-left: 5rem;
        }

        .btn {
          position: absolute;
          right: 0;
          div {
            padding: .5rem;

            ion-icon {
              font-size: 1.2rem;
            }
          }
        }
      }
      .btn-action {
        margin-top: 2rem;
        margin-left: 8%;
        width: 18rem;
        display: flex;
        flex-direction: column;

        .btn {
          margin-top: .5rem;
        }
      }
      .history{
        margin-top: 1rem;
        position: relative;
        width: 30rem;
        cursor: pointer;
        margin-left: -1rem;
        &:after {
          position: absolute;
          content: '';
          width: 70%;
          top: 100%;
          left: 37%;
          transform: translateX(-50%);
          height: 2px;
          background-color: $primary-red;
          transition: .4s;
        }

        &:hover::after {
          width: 50%;

        }
      }

  }
}

</style>