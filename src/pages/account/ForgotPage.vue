<template>
  <headerComp :selected="0"></headerComp>
  <main>
    <section id="sect12">
      <div class="user-group">
        <h2>Mot de passe oublié</h2>
        <form>
          <label for="mot_de_passe">Mot de passe</label>
          <input v-model="password" type="password" name="mot_de_passe" placeholder="Entrez un nouveau mot de passe">

          <label for="mot_de_passe">répéter le Mot de passe</label>
          <input v-model="repeatPassword" type="password" name="mot_de_passe" placeholder="Répétez le nouveau mot de passe">

          <div class="btn-action">
            <div class="btn btn-primary pointer btn-icon-forward" @click="changeForgot">
              <div class="router-link">
                <div>Changer</div>
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
      <WaitingModal :loading="loading" />
    </section>
  </main>
</template>

<script>
import headerComp from "@/components/header/HeaderComp.vue";
import axios from "axios";
import anime from "animejs";
import {IonIcon} from "@ionic/vue";
import ErrorModal from "@/components/modal/ErrorModal.vue";
import ValidateModal from "@/components/modal/ValidateModal.vue";
import WaitingModal from "@/components/modal/WaitingModal.vue";
import SuccessModal from "@/components/modal/SuccessModal.vue";
export default ({
  components: {
    headerComp,
    IonIcon,
    WaitingModal,
    ValidateModal,
    ErrorModal,
    SuccessModal
  },
  data(){
    return {
      errorModalVisible: false,
      errorMessage: "",
      validateModalVisible: false,
      showModalLogin: false,
      loading: false,
      password: '',
      repeatPassword: ''
    }
  },
  methods: {
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
    changeForgot(){
      const token = this.$route.params.token.slice(6);
      this.loading = true;
      axios.post("https://delmoo.fr:5000/forgot/reset", {
        token: token,
        password: this.password,
        repeatPassword: this.repeatPassword
      },{
        validateStatus: function (status) {
          return status === 201 || status === 401 || status === 500 || status === 409;
        }
      }).then((res) => {
        this.loading = false;
        if (res.status == 401 || res.status == 500 || res.status == 409){
          this.showErrorModal(res.data.message)
        }else{
          this.animateModalLogin()
        }
      })
    }
  },
  mounted() {
    const screenHeight = window.innerHeight;
    document.querySelector('#sect12').style.height = screenHeight + "px";
    const token = this.$route.params.token.slice(6);
    axios.post("https://delmoo.fr:5000/forgot/initialize", {
      token: token
    },
        {
          validateStatus: function (status) {
            return status === 201 || status === 401 || status === 500;
          }
        }).then((res) => {
          if (res.status == 500 || res.status == 401){
            this.$router.push('/');
          }
    })
  }
})
</script>

<style lang="scss">
@import '../../utils/computer/components';

main{
  overflow: hidden;
  #sect12 {
    position: relative;
    overflow: hidden;
    padding: 0px;
    display: flex;
    justify-content: center;
    align-items: center;

    &::before {
      position: absolute;
      content: '';
      height: 100%;
      width: 101%;
      bottom: -.5rem;
      background-image: url("../../assets/wave1.svg");
      background-repeat: no-repeat;
      background-position-y: 100%;
      left: 50%;
      transform: translateX(-50%);
    }

    .user-group {
      position: absolute;
      left: 50%;
      top: 50%;
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
          justify-content: center;
          align-items: center;
          margin-top: 2.5rem;
          margin-left: 1rem;
          flex-direction: row;

        }

      }
    }
  }
}

@media only screen and (max-width: 1024px) {
  main {
    #sect12{
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
  }
}

</style>