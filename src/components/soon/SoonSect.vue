<template>
  <section id="sect9" class="common_secondary_pages">
    <div class="infos">
      <h1 txt-title="Prochainement..."></h1>
      <div class="btn btn-primary btn-icon-forward" data-aos="fade-up" data-aos-delay="500" data-aos-duration="800">
        <router-link to="/" class="router-link">
          <div>Retour à l'accueil</div>
          <IonIcon name="chevron-forward"></IonIcon>
        </router-link>
      </div>
    </div>
  </section>
</template>

<script>
import {IonIcon} from "@ionic/vue";
import anime from "animejs";
import AOS from 'aos';

export default {
  components: {
    IonIcon
  },
  mounted() {
    AOS.init();
    const screenHeight = window.innerHeight;
    document.querySelector('#sect9').style.height = screenHeight + "px";
    let titleWrapper = document.querySelector('h1');
    let titleH1 = titleWrapper.getAttribute('txt-title')
    let motH1 = 0;
    titleWrapper.appendChild(document.createElement("div"))

    titleH1.split('').forEach(item => {
      if (item == ' ') {
        titleWrapper.appendChild(document.createElement("div"))
        motH1++;
      } else {
        let span = document.createElement('span')
        span.innerHTML = item
        span.classList.add('letter')
        titleWrapper.children[motH1].appendChild(span)
      }
    });
    anime({
      targets: 'h1 .letter',
      translateX: [40, 0],
      translateZ: 0,
      opacity: [0, 1],
      easing: "easeOutExpo",
      duration: 1600,
      delay: (el, i) => 500 + 30 * i
    });
  }
}
</script>

<style lang="scss">
@import "src/utils/computer/components.scss";

#sect9 {
  padding: 0;
  overflow: hidden;
  &:before {
    position: absolute;
    content: "";
    height: 100%;
    width: 101%;
    bottom: -0.5rem;
    background-image: url('../../assets/wave4.svg');
    background-repeat: no-repeat;
    background-position-y: 100%;
    left: 50%;
    transform: translateX(-50%);
  }


  .infos {
    position: absolute;
    top: 45%;
    left: 50%;
    transform: translate(-50%, -45%);
    text-align: center;
    width: 100%;

    h1 {
      position: relative;
      width: 100%;
      margin: 0;
      margin-bottom: 2rem;
      line-height: 120%;

      > div {
        display: inline-block;

        &:not(div:last-of-type) {
          margin-right: 2rem;
        }

        span {
          font-size: $h2-text;
          color: $primary-red;
          display: inline-block;
        }

      }
    }

    h2 {
      margin: 0;
      margin-bottom: 1.5rem;

      font-size: $h5-text;
      font-weight: 700;
      color: $secondary-red;
    }

    .btn {
      margin-top: .5rem;
    }
  }

}


@media only screen and (max-width: 1024px) {

  // Taille d'écriture
  $small-text: 1.25rem;
  $p-text: 1.5rem;
  $big-text: 1.2rem;
  $h6-text: 1.7rem;
  $h5-text: 2.4rem;
  $h4-text: 2.9rem;
  $h3-text: 3.8rem;
  $h2-text: 3.1rem;
  $h1-text: 6.4rem;

  #sect9 {
    overflow-y: hidden;
    padding: 0;

    .infos {
      position: absolute;
      top: 45%;
      left: 50%;
      transform: translate(-50%, -45%);
      text-align: center;
      width: 100%;

      h1 {
        position: relative;
        width: 100%;
        margin: 0;
        margin-bottom: 2rem;
        line-height: 120%;

        > div {
          display: inline-block;

          &:not(div:last-of-type) {
            margin-right: 2rem;
          }

          span {
            font-size: $h2-text;
            color: $primary-red;
            display: inline-block;
          }

        }
      }

      h2 {
        margin: 0;
        margin-bottom: 1.5rem;

        font-size: $h5-text;
        font-weight: 700;
        color: $secondary-red;
      }

      .btn {
        margin-top: .5rem;
      }
    }

  }

}
</style>