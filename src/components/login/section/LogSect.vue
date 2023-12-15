<template>
  <section id="section4" class="common_login">
    <div class="connexion_inscription">
      <div class="choose_form_header">
        <h2 id="connexion" action="connexion">Se connecter</h2>
        <h2 action="inscription">S'inscrire</h2>
      </div>
      <div class="choose_form_body">
        <div class="connexion">
          <form>
            <label for="etudiant">Email</label>
            <input type="email" name="email" v-model="email" placeholder="Entrez votre email uphf"
                   required>
            <label for="mot_de_passe">Mot de passe</label>
            <input type="password" name="mot_de_passe" v-model="password" placeholder="Entrez votre mot de passe"
                   required>
            <div class="btn-action">
              <div class="btn btn-secondary btn-icon-backward">
                <router-link class="router-link" to="/">
                  <IonIcon name="arrow-back"></IonIcon>
                  <div>Retour</div>
                </router-link>
              </div>
              <div class="btn pointer btn-primary btn-icon-forward btn-submit-connexion" @click="connexion">
                <div>
                  <div>Connexion</div>
                  <IonIcon name="arrow-forward"></IonIcon>
                </div>
              </div>
            </div>
          </form>
        </div>
        <div class="inscription">
          <form>
            <label for="username">Pseudo</label>
            <input v-model="username" type="text" name="username" placeholder="Entrez votre pseudo" required>

            <label for="email">Email (uphf)</label>
            <input v-model="email" type="email" name="email" placeholder="Entrez votre email" required>

            <label for="mot_de_passe">Mot de passe</label>
            <input v-model="password" type="password" name="mot_de_passe" placeholder="Entrez votre mot de passe">

            <label for="mot_de_passe">Confirmation Mot de passe</label>
            <input v-model="repeatPassword" type="password" name="mot_de_passe"
                   placeholder="Repetez votre mot de passe">

            <div class="checkbox">
              <label for="conditions">Accepter les
                <router-link class="router-link" to="/cgu">Conditions Générales
                  <ion-icon name="open"></ion-icon>
                </router-link>
              </label>
              <input type="checkbox" v-model="check" name="conditons">
            </div>

            <div class="btn-action">
              <div class="btn btn-secondary btn-icon-backward">
                <router-link class="router-link" to="/">
                  <IonIcon name="arrow-back"></IonIcon>
                  <div>Retour</div>
                </router-link>
              </div>
              <div class="btn btn-primary pointer btn-icon-forward btn-submit-inscription" @click="register">
                <div>
                  <div>Inscription</div>
                  <IonIcon name="arrow-forward"></IonIcon>
                </div>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
    <ErrorModal :errorModalVisible="errorModalVisible" :message="errorMessage"
                @update:errorModalVisible="updateErrorModal"></ErrorModal>

    <RegisterModal :showModal="showModalResgiter"></RegisterModal>
    <SuccessModal :showModal="showModalLogin"></SuccessModal>
    <LoadingOverlay :loading="loading" />
  </section>
</template>

<script>
import {IonIcon} from '@ionic/vue';
import axios from "axios";
import RegisterModal from "@/components/modal/RegisterModal.vue";
import ErrorModal from "@/components/modal/ErrorModal.vue";
import anime from "animejs";
import SuccessModal from "@/components/modal/SuccessModal.vue";
import LoadingOverlay from "@/components/modal/WaitingModal.vue";

export default {
  components: {
    ErrorModal, RegisterModal, SuccessModal,
    IonIcon,
    LoadingOverlay
  },
  data() {
    return {
      password: '',
      showModalLogin: false,
      showModalResgiter: false,
      errorModalVisible: false,
      errorMessage: "",
      username: '',
      email: '',
      repeatPassword: '',
      check: '',
      loading: false
    }
  },
  methods: {
    animateModalRegister() {
      this.showModalResgiter = true;
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
    show(elt) {
      elt.style.position = 'relative'
      elt.style.visibility = 'visible'
      elt.style.opacity = '1'
      elt.style.maxHeight = '50rem'
    },
    hide(elt) {
      elt.style.position = 'absolute'
      elt.style.opacity = '0'
      elt.style.visibility = 'hidden'
      elt.style.maxHeight = '0'
    },
    showConnexion() {
      let inscriptionForm = document.querySelector('.inscription')
      let inscriptionH2 = document.querySelector('h2[action="inscription"]')

      let connexionForm = document.querySelector('.connexion')
      let connexionH2 = document.querySelector('h2[action="connexion"]')
      connexionH2.classList.add('onForm')
      inscriptionH2.classList.remove('onForm')
      this.hide(inscriptionForm)
      this.show(connexionForm)
    },
    showInscription() {
      let inscriptionForm = document.querySelector('.inscription')
      let inscriptionH2 = document.querySelector('h2[action="inscription"]')

      let connexionForm = document.querySelector('.connexion')
      let connexionH2 = document.querySelector('h2[action="connexion"]')
      connexionH2.classList.remove('onForm')
      inscriptionH2.classList.add('onForm')
      this.show(inscriptionForm)
      this.hide(connexionForm)
    },
    register() {
      this.loading = true;
      if (!this.email || !this.username || !this.password || !this.repeatPassword) {
        this.loading = false;
        this.showErrorModal("Des champs sont manquant");
        return;
      }
      if (this.check != true) {
        this.loading = false;
        this.showErrorModal("Veuillez accepter les cgu");
        return;
      }
      axios.post("https://delmoo.fr:5000/register", {
            password: this.password,
            repeatPassword: this.repeatPassword,
            username: this.username,
            email: this.email
          },
          {
            validateStatus: function (status) {
              return status === 409 || status === 500 || status === 201;
            }
          }).then((res) => {
            this.loading = false;
        if (res.status == 500 || res.status == 409) {
          this.showErrorModal(res.data.message);
          return;
        } else {
          this.animateModalRegister();
        }
      })
    },
    connexion() {
      this.loading = true;
      if (!this.password || !this.email) {
        this.showErrorModal("Veuillez spécifié votre mail et votre mot de passe");
        this.loading = false;

        return;
      }
      axios.post('https://delmoo.fr:5000/login',
          {
            email: this.email,
            password: this.password
          }, {
            withCredentials: true,
            validateStatus: function (status) {
              return status === 409 || status === 500 || status === 201;
            }
          }).then((res) => {
        this.loading = false;

        if (res.status == 500 || res.status == 409) {
          this.showErrorModal(res.data.message);
          return;
        } else {
          this.animateModalLogin();
        }
      })
    }
  },

  mounted() {

    const screenHeight = window.innerHeight;
    const screenWidth = window.innerWidth;

    let main = document.querySelector('main');
    main.style.width = screenWidth + 'px';
    main.style.height = screenHeight + 'px';
    document.querySelector('#section4').style.height = screenHeight + "px";
    this.showInscription();
    let inscriptionH2 = document.querySelector('h2[action="inscription"]')
    let connexionH2 = document.getElementById('connexion');

    connexionH2.addEventListener('click', () => {
      this.showConnexion();
    });
    inscriptionH2.addEventListener('click', () => {
      this.showInscription();
    });
  }

}

</script>

<style lang="scss">
@import "../../../utils/computer/variables";

#section4 {
  position: relative;
  padding: 0px;
  display: flex;
  justify-content: center;
  align-items: center;

  &:before {
    display: none;
  }

  .connexion_inscription {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    overflow: hidden;

    // Effet Glass Morphism
    background: rgba($color: $terciary-white, $alpha: .5); // rgba(35,35,35,0.5)
    -webkit-backdrop-filter: blur(10px);
    backdrop-filter: blur(10px);
    border: 1px solid rgba($color: $terciary-white, $alpha: .25); //rgba(35,35,35,0.25)

    // Effet d'ombre
    box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

    border-radius: $half-round;
    padding: 2.4rem 3rem 2rem 3rem;


    .choose_form_header {
      margin-bottom: 2.5rem;
      display: flex;
      align-items: center;
      justify-content: space-between;


      h2 {
        position: relative;
        font-size: $h5-text;
        color: $secondary-red;
        margin: 0;
        text-transform: uppercase;
        cursor: pointer;

        &:first-of-type {
          margin-right: 3rem;
        }

        &.onForm {
          color: $primary-red;

          &::after {
            position: absolute;
            content: '';
            top: calc(100% + .3rem);
            left: 0;
            width: 100%;
            height: 2px;
            background-color: $primary-red;
            transition: .3s;
          }

        }
      }
    }

    .choose_form_body {

      form, input, label {
        position: relative;
        display: block;
        border: none;
        outline: none;
      }

      form {

        .checkbox {
          display: flex;
          align-items: center;
          margin-top: 1.5rem;

          label {
            .router-link {
              position: relative;
              font-size: $small-text;
              color: $primary-red;

              &:after {
                position: absolute;
                content: '';
                width: 100%;
                top: 100%;
                left: 50%;
                transform: translateX(-50%);
                height: 2px;
                background-color: $primary-red;
                transition: .4s;
              }

              &:hover::after {
                width: 50%;
              }

              ion-icon {
                position: relative;
                font-size: $small-text;
                color: inherit;
                top: .03rem;
              }
            }
          }

          input {
            position: relative;
            margin-left: 1.5rem;
            width: 1rem;
            height: 1rem;
            padding: 0;
            appearance: none;
            -webkit-appearance: none;
            top: -.17rem;

            &::before {
              position: absolute;
              content: '';
              top: 0;
              left: 0;
              height: 100%;
              width: 100%;
              background-color: transparent;
              border: 2px solid $primary-black;
              border-radius: 4px;
              cursor: pointer;
            }

            &::after {
              position: absolute;
              content: '';
              top: 50%;
              left: 50%;
              transform: translate(-33%, -33%);
              height: 0;
              width: 0;
              background-color: $primary-black;
              border-radius: 2px;
              transition: .3s;
              cursor: pointer;
            }

            &:checked::after {
              width: 70%;
              height: 70%;
            }
          }

        }

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
        }

      }

      .connexion, .inscription {
        position: absolute;
        transition: opacity .5s, max-height .4s;

      }
    }

  }
}

</style>