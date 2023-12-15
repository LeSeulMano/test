<template>

  <section class="sect2">
    <div class="about">
      <div class="about-desc">
        <h1 class="title-delmoo"></h1>
        <h2 data-aos="fade-right" data-aos-duration="1000">
          DELMOO, c'est ton asso polyvalente sur le campus ! On partage des trucs pour les cours, on lance des projets
          trop cools, et on anime la vie étudiante avec des podcasts et un journal.
        </h2>
        <h2 data-aos="fade-right" data-aos-delay="200" data-aos-duration="1000">
          Notre but ? Rendre le campus plus
          fun grâce à des events et des ateliers. Peu importe ce que t'aimes, que ce soit l'éducation, la créativité
          ou prendre les devants, chez DELMOO, t'as ta place.
        </h2>
        <div class="about-desc-social">
          <div data-aos="fade-up" data-aos-delay="200" data-aos-duration="1000">
            <a href="https://instagram.com/delmoo_off" target="_blank">
              <div>
                <IonIcon name="logo-instagram"></IonIcon>
              </div>
              <h3>@delmoo_off</h3>
            </a>
          </div>
          <div data-aos="fade-up" data-aos-delay="400" data-aos-duration="1000">
            <a href="mailto:delmoo.pro@gmail.com">
              <div>
                <IonIcon name="mail-outline"></IonIcon>
              </div>
              <h3>delmoo.pro@gmail.com</h3>
            </a>
          </div>
        </div>
      </div>
      <div class="about-illu">
        <div class="about-illu-img about-illu-img-1">
          <img src="../../../assets/about1.jpg">
          <div class="side-bar"></div>
        </div>
        <div class="about-illu-img about-illu-img-2">
          <img src="../../../assets/about2.jpg">
          <div class="side-bar"></div>
        </div>
      </div>
    </div>
  </section>

</template>

<script>
import anime from 'animejs';
import { IonIcon } from "@ionic/vue";
export default {
  name: "AboutSect",
  components: {
    IonIcon
  },
  methods: {

    cumulativeOffset(element) {
      var top = 0, left = 0;
      do {
        top += element.offsetTop || 0;
        left += element.offsetLeft || 0;
        element = element.offsetParent;
      } while (element);

      return {
        top: top,
        left: left
      };
    },

  },

  mounted() {
    // Animation titre sect2
    let titleWrapper = document.querySelector('.title-delmoo');
    let titleH1 = "Delmoo, c'est quoi ?"
    let motH1 = 0;
    titleWrapper.appendChild(document.createElement("div"))
    const h1DelmooTop = this.cumulativeOffset(document.querySelector('.title-delmoo')).top;

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

    const h1DelmooAnimation = anime({
      targets: '.title-delmoo .letter',
      translateX: [40, 0],
      translateZ: 0,
      opacity: [0, 1],
      easing: "easeOutExpo",
      duration: 1600,
      autoplay: false,
      delay: (el, i) => 500 + 30 * i
    });

    document.addEventListener("scroll", () => {
      if (document.documentElement.scrollTop * 2 > h1DelmooTop && !titleWrapper.classList.contains('displayed')) {
        h1DelmooAnimation.play()
        titleWrapper.classList.add('displayed')
      }
    })
  }
}

</script>

<style lang="scss">
@import "../../../utils/computer/variables";

.sect2 {

  background-color: $primary-red;
  position: relative;
  min-height: 60vh;

  a {
    text-decoration: none;
  }

  .about {
    position: relative;
    width: 80%;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    align-items: center;

    h1, h2 {
      color: $primary-white;
    }

    .about-desc {
      width: 52%;

      h1 {
        font-size: $h1-text;
        margin: 0;
        margin-bottom: 3.5rem;
        line-height: 120%;

        div {
          display: inline-block;
          margin-right: 1.5rem;
          font-size: $h1-text;
          white-space: nowrap;

          span {
            display: inline;
            font-size: $h1-text;
          }
        }

        .letter {
          display: inline-block;
          font-size: $h1-text;
          color: $primary-white;
          opacity: 0;
        }
      }

      h2 {
        font-size: $p-text;
        font-weight: 400;
      }

      h2:not(p:last-of-type) {
        margin-bottom: 1.6rem;
      }

      .about-desc-social {
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        margin-top: 2rem;

        > div {

          &:not(div:last-of-type) {
            margin-right: 3.5rem;
          }

          a {
            position: relative;
            display: flex;
            align-items: center;

            h3, ion-icon {
              color: $primary-white;
              margin: 0;
            }

            h3 {
              position: relative;
              font-size: $p-text;
              font-weight: 600;

              &:after {
                position: absolute;
                content: '';
                height: 2px;
                width: 0%;
                top: 100%;
                left: 0;
                transition: .3s;
                background-color: $primary-white;
                z-index: 2;
              }
            }

            ion-icon {
              position: relative;
              top: .22rem;
              font-size: $h5-text;
              margin-right: .5rem;
            }

            &:hover h3::after {
              width: 100%;
            }
          }
        }
      }
    }

    .about-illu {
      position: relative;
      width: 45%;
      height: 40vw;

      .about-illu-img {
        position: absolute;
        width: 16vw;
        height: 16vw;

        box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

        img {
          width: 100%;
          height: 100%;
          border-radius: $half-round;
        }

        &.about-illu-img-1 {
          z-index: 3;
          top: 30%;
          left: 23%;
          transform: translate(-23%, -30%);

          // Ligne sur le côté
          &:after, &:before {
            position: absolute;
            content: '';
            top: 2%;
            width: 5px;
            background-color: $primary-white;
            transform: skewY(30deg);
          }

          &:after {
            right: -1.8vh;
            height: 14vh;
          }

          &:before {
            right: -3.1vh;
            height: 10vh;
          }
        }

        &.about-illu-img-2 {
          z-index: 2;
          top: 85%;
          left: 90%;
          transform: translate(-90%, -85%);

          // Ligne sur le côté
          &:after, &:before {
            position: absolute;
            content: '';
            bottom: 2%;
            width: 5px;
            background-color: $primary-white;
            transform: skewY(30deg);
          }

          &:after {
            left: -1.8vh;
            height: 14vh;
          }

          &:before {
            left: -3.1vh;
            height: 10vh;
          }
        }
      }
    }

  }

}

@media only screen and (max-width: 1024px) {

  $small-text: 1.25rem;
  $p-text: 1.2rem;
  $big-text: 1.2rem;
  $h6-text: 1.3rem;
  $h5-text: 1.8rem;
  $h4-text: 2.9rem;
  $h3-text: 3.8rem;
  $h2-text: 4.9rem;
  $h1-text: 4rem;


  .sect2 {
    background-color: $primary-red;


    .about {
      padding: 3.5rem 0;
      width: 80%;

      h1, h2 {
        color: $primary-white;
      }

      .about-desc {
        width: 100%;

        h1 {
          font-size: $h1-text;
          line-height: 100%;
          margin: 0;
          margin-bottom: 3.8rem;

          .letter {
            font-size: $h1-text;
            color: $primary-white;
            opacity: 0;
          }
        }

        h2 {
          font-size: $p-text;
        }

        h2:not(p:last-of-type) {
          margin-bottom: 2rem;
        }

        .about-desc-social {
          margin-top: 2rem;

          > div {

            &:not(div:last-of-type) {
              margin-right: 0;
              margin-bottom: .8rem;
            }

            a {
              h3 {
                font-size: $h6-text;
              }

              ion-icon {
                top: .37rem;
                font-size: $h5-text;
                margin-right: .8rem;
              }

            }
          }
        }
      }

      .about-illu {
        position: relative;
        width: 100%;
        height: 30rem;

        .about-illu-img {
          position: absolute;
          width: 13.5rem;
          height: 13.5rem;

          box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

          img {
            width: 100%;
            height: 100%;
            border-radius: $half-round;
          }

          &.about-illu-img-1 {
            z-index: 3;
            top: 25%;
            left: 0%;
            transform: translate(-0%, -25%);

            // Ligne sur le côté
            &:after, &:before {
              position: absolute;
              content: '';
              top: 2%;
              width: 2px;
              background-color: $primary-white;
              transform: skewY(30deg);
            }

            &:after {
              right: -1.1vh;
              height: 10vh;
            }

            &:before {
              right: -2vh;
              height: 6.3vh;
            }
          }

          &.about-illu-img-2 {
            z-index: 2;
            top: 87%;
            left: 100%;
            transform: translate(-100%, -85%);

            // Ligne sur le côté
            &:after, &:before {
              position: absolute;
              content: '';
              bottom: 2%;
              width: 2px;
              background-color: $primary-white;
              transform: skewY(30deg);
            }

            &:after {
              left: -1.1vh;
              height: 10vh;
            }

            &:before {
              left: -2vh;
              height: 6.3vh;
            }
          }
        }
      }

    }
  }

}

</style>