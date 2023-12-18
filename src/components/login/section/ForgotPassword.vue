<template>
  <div class="user-group" v-if="isForgot">
    <h2>Mot de passe oublié</h2>
    <form>

      <label for="mot_de_passe">Email</label>
      <input v-model="email" type="email" name="mot_de_passe" placeholder="Entrez votre adresse mail">

      <div class="btn-action">
        <div class="btn btn-secondary btn-icon-backward pointer" @click="emitInfoUser">
          <div class="router-link">
            <IonIcon name="arrow-back"></IonIcon>
            <div>Retour</div>
          </div>
        </div>
        <div class="btn btn-primary pointer btn-icon-forward" @click="change">
          <div class="router-link">
            <div>Envoyé</div>
            <IonIcon name="arrow-forward"></IonIcon>
          </div>
        </div>
      </div>
    </form>
  </div >
  <ErrorModal :errorModalVisible="errorModalVisible" :message="errorMessage"
              @update:errorModalVisible="updateErrorModal"></ErrorModal>
  <ValidateModal :validateModalVisible="validateModalVisible" @update:validateModalVisible="updateValidateModal"></ValidateModal>
  <SuccessModal :showModal="showModalLogin"></SuccessModal>
  <LoadingOverlay :loading="loading" />
</template>

<script >
import {IonIcon} from "@ionic/vue";
import SuccessModal from "@/components/modal/SuccessModal.vue";
import ValidateModal from "@/components/modal/ValidateModal.vue";
import ErrorModal from "@/components/modal/ErrorModal.vue";
import anime from "animejs";
import axios from "axios";
import LoadingOverlay from "@/components/modal/WaitingModal.vue";

export default {
  components: {
    LoadingOverlay,
    IonIcon,
    SuccessModal,
    ValidateModal,
    ErrorModal
  },
  data(){
    return {
      errorModalVisible: false,
      errorMessage: "",
      validateModalVisible: false,
      email: '',
      showModalLogin: false,
      loading: false,
    }
  },
  emits: ['change-tab'],
  methods: {
    emitInfoUser() {
      this.$emit('change-tab', 'InfoUser');
    },
    animateModalLogin() {
      this.showModalLogin = true;
      anime({
        targets: '.modal-content',
        scale: [0, 1],
        opacity: [0, 1],
        easing: 'easeOutQuad',
        duration: 500,
        delay: 300,
        complete: () => {
          setTimeout(() => {
            this.$router.push('/');
          }, 1000);
        },
      });
    },
    change(){
      this.loading = true;
      axios.post('https://delmoo.fr:5000/forgot', {
        email: this.email
      },{
        withCredentials: true,
        validateStatus: function (status) {
          return status === 409 || status === 500 || status === 201;
        }
      }).then((res) => {
        this.loading = false;
        if (res.status == 409 || res.status == 500) {
          this.showErrorModal(res.data.message)
        }else{
          this.animateModalLogin();
        }
      })
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
  },
  props: {
    isForgot: Boolean
  }

}
</script>

<style lang="scss">
@import '../../../utils/computer/components.scss';

.user-group {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  display: none;
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


  form, input, label {
    position: relative;
    display: block;
    border: none;
    outline: none;
  }

  form {

    label {
      font-size: $small-text;
      text-transform: uppercase;
      color: $primary-red;
      font-weight: 600;
      text-indent: .5rem;
    }

    input {
      font-size: $p-text;
      color: $primary-black;
      padding: .8rem 1rem;
      border-radius: $half-round;
      width: 18rem;

      &::placeholder {
        color: $secondary-black;
      }

      margin-bottom: 1.3rem;
    }

    .btn-action {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 2.5rem;
      margin-left: 1rem;
      flex-direction: row;

      .btn{
        margin-right: 2rem;
      }
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

    form {
      .btn-action {
        .btn {
          ion-icon {
            font-size: 1.5rem;
          }
        }
      }

    }
  }
}
</style>