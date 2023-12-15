<template>
  <div class="file">
    <div class="file-metadata">
      <div class="file-type btn btn-cours btn-no-hover file-type-cm">
        <div>
          <div>{{ type }}</div>
        </div>
      </div>
      <div class="file-date-added">{{ date }}</div>
    </div>
    <h4>{{ matter }}</h4>
    <h5>{{ name }}</h5>
    <div class="file-cta">
      <a :href="getFile(path)" target="_blank">
        <div class="btn btn-primary btn-icon-forward pointer">
          <div>
            <div>Voir</div>
            <IonIcon name="eye"></IonIcon>
          </div>
        </div>
      </a>
      <div class="btn btn-cours btn-icon pointer" @click="downloadFile(path)">
        <div>
          <div>
            <IonIcon name="cloud-download"></IonIcon>
          </div>
        </div>
      </div>
    </div>
  </div>
  <ErrorModal :errorModalVisible="errorModalVisible" :message="errorMessage"
              @update:errorModalVisible="updateErrorModal"></ErrorModal>
</template>

<script>
import {IonIcon} from '@ionic/vue';
import axios from "axios";
import ErrorModal from "@/components/modal/ErrorModal.vue";
import anime from "animejs";

export default ({
  components: {
    IonIcon,
    ErrorModal
  },
  methods: {
    getFile(path) {
      return `https://delmoo.fr:5000/cours/open?file=${path}`;
    },
    downloadFile(coursPath) {
      axios({
        method: 'get',
        url: 'https://delmoo.fr:5000/cours/download',
        params: {
          path: coursPath,
        },
        responseType: 'blob',
        validateStatus: function (status) {
          return status === 200 || status === 500 || status === 403;
        }
      })
          .then((response) => {
            const blob = new Blob([response.data], { type: response.headers['content-type'] });

            const link = document.createElement('a');
            link.href = window.URL.createObjectURL(blob);

            if (response.headers['content-disposition']) {
              link.download = response.headers['content-disposition'].split('=')[1];
            } else {
              link.download = 'downloaded_file';
            }

            document.body.appendChild(link);

            link.click();

            document.body.removeChild(link);
          })
          .catch(error => {
            console.error('Erreur lors du téléchargement du fichier :', error);
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
  },
  data() {
    return {
      errorModalVisible: false,
      errorMessage: "",
    }
  },
  props: {
    type: String,
    matter: String,
    name: String,
    date: Number,
    path: String
  },


})
</script>

<style lang="scss">
@import "../../../../utils/computer/components.scss";

.file {
  position: relative;
  margin-right: 3%;
  margin-bottom: 3%;
  padding: 1.5rem;
  border-radius: $half-round;
  width: 22%;
  min-width: 170px;

  // Effet Glass Morphism
  background: rgba($color: $terciary-white, $alpha: .5); // rgba(35,35,35,0.5)
  -webkit-backdrop-filter: blur(10px);
  backdrop-filter: blur(10px);
  border: 1px solid rgba($color: $terciary-white, $alpha: .25); //rgba(35,35,35,0.25)

  // Effet d'ombre
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

  .file-metadata {
    display: flex;
    justify-content: space-between;

    .file-type {
      display: inline-block;

      > div {
        padding: .4rem .6rem;

        > div {
          font-size: $small-text;
          color: $primary-white;
          font-weight: 600;
          text-transform: uppercase;
        }
      }
    }

    .file-date-added {
      padding: .4rem .6rem;
      font-size: $small-text;
      color: $primary-white;
      font-weight: 600;
    }
  }

  h4 {
    display: block;
    font-size: $h5-text;
    color: $primary-white;
    text-align: center;
    font-weight: 700;
    margin: 1.2rem 0 .4rem 0;
    width: 100%;

    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }

  h5 {
    font-size: $p-text;
    font-weight: 600;
    margin: 0;
    margin-bottom: 1rem;
    color: $primary-white;
    text-align: center;
  }

  .file-cta {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 1.4rem;

    .btn {
      margin-right: 1rem;

      > div {
        display: flex;
        align-items: center;
        padding: 0.55rem 0.9rem;

        ion-icon {
          font-size: $h6-text;
        }
      }

      &.btn-icon-forward {
        margin-left: 1rem;
      }

      &.btn-icon {
        > div {
          padding: 0.82rem 0.9rem 0.28rem 0.9rem;
        }
      }

    }
  }
}
</style>