<template>
  <section class="sect3">
    <div class="team">
      <h1 class="title-sect3"></h1>
      <div class="team-members">
        <StaffMembers url="Yacine.png" name="Yacine" role="Président"></StaffMembers>
        <StaffMembers url="Marin.png" name="Marin" role="Vice Président"></StaffMembers>
        <StaffMembers url="Nathan.png" name="Nathan" role="Trésorier"></StaffMembers>
        <StaffMembers url="Gaby.png" name="Gaby" role="Secrétaire"></StaffMembers>
        <StaffMembers url="Noa.png" name="Noa" role="Co-Secrétaire"></StaffMembers>
      </div>
      <div class="team-cta">
        <div class="btn btn-primary btn-icon-forward" data-aos="fade-up" data-aos-delay="400" data-aos-duration="600">
          <a class="router-link" href="https://instagram.com/delmoo_off" target="_blank">
            <div>Nous rejoindre</div>
            <IonIcon name="people"></IonIcon>
          </a>
        </div>
        <div class="btn btn-cours btn-icon-forward" data-aos="fade-up" data-aos-delay="600" data-aos-duration="600">
          <a class="router-link" href="https://forms.gle/jftwurkpbc1Ambr2A">
            <div>Devenir tuteur</div>
            <IonIcon name="school"></IonIcon>
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import StaffMembers from "@/components/home/sections/cards/StaffMembers.vue";
import anime from "animejs";
import { IonIcon } from '@ionic/vue';

export default {
  name: "ArticleSect",
  components: {
    StaffMembers,
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

    let titleSect3Wrapper = document.querySelector('.title-sect3');
    let titleSect3H1 = "Notre équipe"
    let motSect3H1 = 0;
    titleSect3Wrapper.appendChild(document.createElement("div"))

    titleSect3H1.split('').forEach(item => {
      if (item == ' ') {
        titleSect3Wrapper.appendChild(document.createElement("div"))
        motSect3H1++;
      } else {
        let span = document.createElement('span')
        span.innerHTML = item
        span.classList.add('letter')
        titleSect3Wrapper.children[motSect3H1].appendChild(span)
      }
    });

    const h1Sect3Animation = anime({
      targets: '.title-sect3 .letter',
      translateX: [40, 0],
      translateZ: 0,
      opacity: [0, 1],
      easing: "easeOutExpo",
      duration: 1600,
      autoplay: false,
      delay: (el, i) => 500 + 30 * i
    });
    const h1Sect3Top = this.cumulativeOffset(document.querySelector('.title-sect3')).top;

    document.addEventListener("scroll", () => {
      // Animation du titre sect3
      if (document.documentElement.scrollTop * 2 > h1Sect3Top && !titleSect3Wrapper.classList.contains('displayed')) {
        h1Sect3Animation.play()
        titleSect3Wrapper.classList.add('displayed')
      }
    })

  }
}

</script>

<style lang="scss">
@import "../../../utils/computer/variables";

.sect3 {
  z-index: 2;
  position: relative;

  &:before {
    position: absolute;
    content: "";
    height: 100%;
    width: 101%;
    bottom: -0.5rem;
    background-image: url("../../../assets/wave4.svg");
    background-repeat: no-repeat;
    background-position-y: 100%;
    left: 50%;
    transform: translateX(-50%);
  }

  &:after {
    position: absolute;
    content: "";
    height: 100%;
    width: 101%;
    top: -0.5rem;
    background-image: url("../../../assets/wave2.svg");
    background-repeat: no-repeat;
    left: 50%;
    transform: translateX(-50%);
    z-index: -1;

  }

  .team {
    position: relative;
    width: 90%;
    margin: 0 auto;
    padding-top: 17rem;

    h1 {
      line-height: 120%;
      margin: 0;
      font-size: $h1-text;

      div {
        display: inline-block;
        margin-right: 1.5rem;
        white-space: nowrap;

        span {
          display: inline;
          font-size: $h1-text;
        }
      }

      .letter {
        display: inline-block;
        font-size: $h1-text;
        color: $primary-red;
        opacity: 0;
      }
    }

    .team-cta {
      position: relative;
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      margin-top: 4rem;

      .btn {
        &:not(.btn:last-of-type) {
          margin-right: 3rem;
        }
      }
    }
  }
}

@media only screen and (max-width: 1024px) {
  $small-text: 1.25rem;
  $p-text: 1.5rem;
  $big-text: 1.2rem;
  $h6-text: 1.7rem;
  $h5-text: 2.4rem;
  $h4-text: 2.9rem;
  $h3-text: 3.8rem;
  $h2-text: 4.9rem;
  $h1-text: 4rem;

  .sect3 {
    &:after {
      background-repeat: no-repeat;
      background-position-y: 0;
    }

    .team {
      padding-top: 12rem;

      h1 {
        font-size: $h1-text;
        line-height: 100%;

        .letter {
          font-size: $h1-text;
        }
      }


      .team-cta {
        margin-top: 2.5rem;

        .btn {
          margin-bottom: 2.5rem;

          &:not(.btn:last-of-type) {
            margin-right: 3rem;
            margin-left: 3rem;
          }
        }
      }
    }
  }

}
</style>