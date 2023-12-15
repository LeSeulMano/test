<template>
  <section id="sect11">
    <div class="user-group">
      <h2>{{ information.username }} : </h2>
      <h2 v-if="information.is_checked == 0" class="warning">Attention vous n'avez pas vérifié votre adresse mail uphf !</h2>
      <div class="user-information">
        <h3>Adresse mail: </h3>
        <p>{{ information.email }}</p>
      </div>
      <div class="user-information">
        <h3>Nom d'utillisateur: </h3>
        <p>{{ information.username }}</p>
        <div class="btn pointer btn-primary btn-icon">
          <div>
            <IonIcon name="pencil-outline"></IonIcon>
          </div>
        </div>
      </div>
      <div class="btn-action">
        <div class="btn pointer btn-primary btn-icon">
          <div>
            <div>Changer de mot de passe</div>
          </div>
        </div>
        <div class="btn pointer btn-secondary btn-icon">
          <div>
            <div>Supprimer le compte</div>
          </div>
        </div>
      </div>

    </div>
    <ErrorModal :errorModalVisible="errorModalVisible" :message="errorMessage"
                @update:errorModalVisible="updateErrorModal"></ErrorModal>
  </section>
</template>

<script>
import axios from "axios";
import anime from "animejs";
import ErrorModal from "@/components/modal/ErrorModal.vue";
import {IonIcon} from "@ionic/vue";
export default {
  components: {
    IonIcon,
    ErrorModal
  },
  data(){
    return{
      information: {}
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
      if (res.status == 201){
        this.information = res.data.json[0];
      } else {
        this.showErrorModal(res.data.message);
        return;
      }
    })
  },
  methods: {
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
  }
}
</script>

<style lang="scss">
@import "../../../utils/computer/components.scss";
#sect11{
  position: relative;
  overflow: hidden;
  padding: 0px;

  &::before {
    position: absolute;
    content: '';
    height: 100%;
    width: 101%;
    bottom: -.5rem;
    background-image: url("../../../assets/login1.svg");
    background-size: 100%;
    background-repeat: no-repeat;
    background-position-y: 50%;
    left: 50%;
    transform: translateX(-50%);
  }

  .user-group{
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

    h2{
      text-align: center;
    }

    .warning{
      color: $primary-red;
    }

    .user-information{
      display: flex;
      justify-content: space-between;
      position: relative;

      p{
        margin-right: 10rem;
      }

      .btn{
        position: absolute;
        right: 0;
      }
    }

    .btn-action{
      margin-top: 2rem;
      margin-left: 30%;
      width: 18rem;
      display: flex;
      flex-direction: column;
      .btn{
        margin-top: .5rem;
      }
    }

  }

}
</style>