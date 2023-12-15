<template>
  <section class="sect1" id="sect1">
    <div class="news">
      <div class="article" data-aos="fade-right" data-aos-delay="100" data-aos-duration="1000">
        <div class="btn btn-podcast type-article btn-no-hover">
          <div>
            <div>podcast</div>
          </div>
        </div>
        <h1>Présentation de l'équipe Delmoo !</h1>
        <div class="img-article"><img src="../../../assets/podcast.jpeg"></div>
        <div class="infos-article">
          <div class="info-article">
            <IonIcon name="time"></IonIcon>
            <div>12:23</div>
          </div>
          <div class="info-article">
            <IonIcon name="person"></IonIcon>
            <div>Yacine, Marin et toute la clique</div>
          </div>
        </div>
        <div class="btn btn-primary btn-icon-forward">
          <router-link class="router-link" to="/podcast">
            <div>Regarder</div>
            <IonIcon name="play-circle"></IonIcon>
          </router-link>
        </div>
      </div>

      <div class="article" data-aos="fade-up" data-aos-delay="100" data-aos-duration="1000">
        <div class="btn btn-shop type-article btn-no-hover">
          <div>
            <div>shop</div>
          </div>
        </div>
        <h1>Découvrez les pulls & les t-shirt en édition limité</h1>
        <div class="img-article"><img src="../../../assets/shop.webp"></div>
        <div class="infos-article">
          <div class="info-article">
            <IonIcon name="calendar-number"></IonIcon>
            <div>21.09.2023</div>
          </div>
          <div class="info-article">
            <IonIcon name="shirt"></IonIcon>
            <div>Pulls, t-shirt, bob</div>
          </div>
        </div>
        <div class="btn btn-primary btn-icon-forward">
          <router-link class="router-link" to="/shop">
            <div>Découvrir</div>
            <IonIcon name="chevron-forward"></IonIcon>
          </router-link>
        </div>
      </div>

      <div class="article" data-aos="fade-left" data-aos-delay="100" data-aos-duration="1000">
        <div class="btn btn-cours type-article btn-no-hover">
          <div>
            <div>cours</div>
          </div>
        </div>
        <h1>Révise tes partiels avec les nouveaux cours</h1>
        <div class="img-article"><img src="../../../assets/cours.webp"></div>
        <div class="infos-article">
          <div class="info-article">
            <IonIcon name="calendar-number"></IonIcon>
            <div>19.09.2023</div>
          </div>
          <div class="info-article">
            <IonIcon name="document-text"></IonIcon>
            <div>Algèbre, Analyse, Méca.</div>
          </div>
        </div>
        <div class="btn btn-primary btn-icon-forward">
          <router-link class="router-link" to="/cours">
            <div>Voir les cours</div>
            <IonIcon name="pencil"></IonIcon>
          </router-link>
        </div>
      </div>
      <div class="carousel-dot">
        <div class="on-carousel-dot"></div>
        <div></div>
        <div></div>
      </div>
    </div>
    <div class="scroll-next-section">
      <p>Découvrez qui nous sommes !</p>
      <span class="material-icons">expand_more</span>
    </div>
  </section>

</template>

<script>
import AOS from 'aos';
import Hammer from 'hammerjs';
import { IonIcon } from '@ionic/vue';
export default {
  name: 'LandingPage',
  components: {
    IonIcon
  },
  data() {
    const screenHeight = window.innerHeight;
    return {
      screenHeight,
      arrayCarousel: ['0', '-31.85rem', "-62.7rem"],
      articleListItems: [],
      bulletListItems: [],
      onSlideNb: 0,
      temps: 5,
      intervalTimerTouch: null,
      intervalCarousel: null,
      onSlide: false,
    }
  },
  methods: {
    cumulativeOffset(element) {
      let top = 0;
      let left = 0;
      do {
        top += element.offsetTop || 0;
        left += element.offsetLeft || 0;
        element = element.offsetParent;
      } while (element);

      return {
        top: top,
        left: left,
      };
    },
    carouselRight() {
      this.articleListItems.forEach((item) => {
        item.style.transform = 'translate(' + this.arrayCarousel[(this.onSlideNb + 1) % 3] + ')';
      });

      this.bulletListItems.forEach((item) => {
        item.classList.remove('on-carousel-dot');
      });
      this.bulletListItems[(this.onSlideNb + 1) % 3].classList.add('on-carousel-dot');

      this.onSlideNb++;
    },
    carouselLeft() {
      this.articleListItems.forEach((item) => {
        item.style.transform = 'translate(' + this.arrayCarousel[(this.onSlideNb - 1) % 3] + ')';
      });

      this.bulletListItems.forEach((item) => {
        item.classList.remove('on-carousel-dot');
      });
      this.bulletListItems[(this.onSlideNb - 1) % 3].classList.add('on-carousel-dot');

      this.onSlideNb--;
    },
    checkTimeTouch() {
      this.temps--;
      if (this.temps === 0) {
        this.intervalCarousel = setInterval(this.carouselRight, 3000);
        clearInterval(this.intervalTimerTouch);
      }
    },
    initializeCarousel() {
      if (window.matchMedia('(max-width: 1024px)').matches) {
        this.intervalCarousel = setInterval(this.carouselRight, 3000);

        // Swipe vers la gauche
        const hammer = new Hammer(this.$el);
        hammer.on('panleft', () => {
          if (!this.onSlide) {
            if (this.onSlideNb % 3 < 2) {
              clearInterval(this.intervalCarousel);
              clearInterval(this.intervalTimerTouch);

              this.carouselRight();

              this.temps = 5;
              this.intervalTimerTouch = setInterval(this.checkTimeTouch, 1000);
            }

            this.onSlide = true;
            setTimeout(() => {
              this.onSlide = false;
            }, 300);
          }
        });

        // Swipe vers la droite
        hammer.on('panright', () => {
          if (!this.onSlide) {
            if (this.onSlideNb % 3 > 0 || this.onSlideNb === 3) {
              clearInterval(this.intervalCarousel);
              clearInterval(this.intervalTimerTouch);

              this.carouselLeft();

              this.temps = 5;
              this.intervalTimerTouch = setInterval(this.checkTimeTouch, 1000);
            }

            this.onSlide = true;
            setTimeout(() => {
              this.onSlide = false;
            }, 300);
          }
        });
      }
    },
  },
  mounted() {
    AOS.init();
    document.querySelector('#sect1').style.height = this.screenHeight + "px";
    const scrollNextSection = document.querySelector(".scroll-next-section");
    document.addEventListener("scroll", () => {
      if (document.documentElement.scrollTop > 50) {
        scrollNextSection.style.opacity = '0';
      } else {
        scrollNextSection.style.opacity = '1';
      }
    })
    this.articleListItems = document.querySelectorAll('.article');
    this.bulletListItems = document.querySelectorAll('.carousel-dot div');
    this.initializeCarousel();
  }
}
</script>

<style lang="scss">
@import '../../../utils/computer/components.scss';

section {
  position: relative;
}

.sect1 {

  z-index: 2;
  width: 100%;
  overflow: hidden;

  &:after {
    position: absolute;
    content: '';
    height: 100%;
    width: 101%;
    bottom: -.5rem;
    background-image: url("../../../assets/wave1.svg");
    background-repeat: no-repeat;
    background-position-y: 100%;
    left: 50%;
    transform: translateX(-50%);
    z-index: -1;
  }


  .news {
    position: absolute;
    width: 75%;
    left: 50%;
    top: 59%;
    transform: translate(-50%, -59%);
    display: flex;
    justify-content: space-between;
    align-items: center;

    .article {
      // Ligne sur le côté
      &:after, &:before {
        position: absolute;
        content: '';
        top: .5rem;
        width: 5px;
        background-color: $primary-red;
        transform: skewY(30deg);
      }

      &:after {
        left: -.9rem;
        height: 7rem;
      }

      &:before {
        left: -1.7rem;
        height: 4.7rem;
      }

      // Effet Glass Morphism
      background: rgba($color: $terciary-white, $alpha: .5); // rgba(35,35,35,0.5)
      -webkit-backdrop-filter: blur(10px);
      backdrop-filter: blur(10px);
      border: 1px solid rgba($color: $terciary-white, $alpha: .25); //rgba(35,35,35,0.25)

      // Effet d'ombre
      box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

      border-radius: $half-round;
      padding: 1.3rem 1.8rem 1.3rem 1.8rem;
      width: 24%;

      .type-article {
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

      h1 {
        display: block;
        font-size: $h4-text;
        margin: .8rem 0 1rem 0;
        width: 100%;
        line-height: 133%;

        // Permet de couper un texte trop long si nécessaire
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
      }

      .img-article {
        position: relative;
        border-radius: $half-round;
        overflow: hidden;
        width: 100%;
        aspect-ratio: 16 / 10;

        img {
          position: absolute;
          left: 50%;
          top: 50%;
          transform: translate(-50%, -50%);
          width: 100%;
        }
      }

      .infos-article {
        margin-top: .5rem;
        display: flex;
        justify-content: space-between;

        .info-article {
          display: flex;
          align-items: center;
          justify-content: space-between;
          width: 50%;

          div {
            width: 100%;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            margin-left: .4rem;
          }

          ion-icon {
            font-size: $h6-text;
          }
        }
      }

      .btn-primary {
        margin-top: 1.5rem;
        left: 50%;
        transform: translateX(-50%);
      }
    }

    .carousel-dot {
      display: none;
    }
  }

  .scroll-next-section {
    position: absolute;
    left: 50%;
    top: 98.5%;
    transform: translate(-50%, -98.5%);
    text-align: center;
    transition: .3s;

    p {
      font-size: $p-text;
      color: $primary-white;
      font-weight: 600;
      margin: 0;
      margin-bottom: .1rem;
    }

    .material-icons {
      font-size: $h5-text;
      color: $primary-white;
      animation: updown 3s ease infinite;
      line-height: .5rem;
    }
  }

}

@media only screen and (max-width: 1024px) {

  $small-text: 0.9rem;
  $p-text: 1.5rem;
  $big-text: 1.2rem;
  $h6-text: 1.7rem;
  $h5-text: 2.4rem;
  $h4-text: 2.7rem;
  $h3-text: 3.8rem;
  $h2-text: 4.9rem;
  $h1-text: 6.4rem;

  .sect1 {

    &:after {
      background-repeat: no-repeat;
      background-position-y: 100%;
      background-position-x: 0%;
      background-size: 120%;
    }

    .news {
      position: absolute;
      width: 70%;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      display: flex;
      justify-content: space-between;
      align-items: center;

      .article {

        padding: 1.3rem 1.3rem 1.3rem 1.3rem;
        margin-left: -1.7rem;
        width: 100%;
        margin-right: 10rem;
        transition: .9s;

        &:after, &:before {
          position: absolute;
          content: '';
          top: .5rem;
          width: 2px;
          background-color: $primary-red;
          transform: skewY(30deg);
        }

        &:after {
          left: -.6rem;
          height: 4.7rem;
        }

        &:before {
          left: -1.2rem;
          height: 3rem;
        }

        .type-article {
          > div {
            padding: .5rem .6rem;

            > div {
              font-size: $small-text;
            }

          }

        }

        h1 {
          font-size: $h4-text;
          margin: 0.9rem 0 1.7rem 0;
        }

        .img-article {
          width: 68vw;
          height: 13rem;

          img {
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            width: 100%;
          }
        }

        .infos-article {
          margin-top: .5rem;

          .info-article {
            div {
              margin-left: .5rem;
              font-size: 1rem;
            }

            ion-icon {
              font-size: $h6-text;
            }
          }
        }

        .btn-primary {
          margin-top: 2rem;
        }

      }

      .carousel-dot {
        position: absolute;
        bottom: -2.5rem;
        left: 50%;
        transform: translateX(-50%);
        display: flex;

        div {
          transition: .4s;
          width: 0.7rem;
          height: 0.7rem;

          border-radius: $round;
          border: 1px solid $primary-red;
          background-color: transparent;
          box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

          &:not(div:last-of-type) {
            margin-right: 1rem;
          }
        }

        .on-carousel-dot {
          background-color: $primary-red !important;
        }
      }

    }

    .scroll-next-section {
      left: 50%;
      top: 98%;
      transform: translate(-50%, -98%);

      p {
        font-size: $h6-text;
        margin-bottom: .3rem;
        display: none;
      }

      .material-icons {
        font-size: $h4-text;
      }
    }
  }

}

@keyframes updown {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(.4rem);
  }
}
</style>