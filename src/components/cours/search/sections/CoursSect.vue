<template>
  <section class="sect1-cours" id="sect1">
    <div class="courses">
      <div class="search-courses">
        <div class="input-search-courses" data-aos="fade-up" data-aos-delay="150" data-aos-duration="500">
          <input type="text" v-model="search" placeholder="Quel cours recherches-tu ?">
          <router-link class="router-link div" :to="{ name: 'Result', query: {search: search}}">
            <IonIcon name="search"></IonIcon>
          </router-link>
        </div>
      </div>
      <div class="courses-shortcut">
        <div class="all-courses">
          <div class="btn btn-cours pointer" data-aos="fade-up" data-aos-delay="500" data-aos-duration="500">
            <router-link class="router-link" :to="{ name: 'Result', query: {search: '1'}}">1A</router-link>
          </div>
          <div class="btn btn-cours pointer" data-aos="fade-up" data-aos-delay="650" data-aos-duration="500">
            <router-link class="router-link" :to="{ name: 'Result', query: {search: '2'}}">2A</router-link>
          </div>
          <div class="btn btn-cours pointer" data-aos="fade-up" data-aos-delay="800" data-aos-duration="500">
            <router-link class="router-link" :to="{ name: 'Result', query: {result: '3'}}">3A</router-link>
          </div>
        </div>
        <div class="btn btn-primary btn-icon-forward adding-course" data-aos="fade-up" data-aos-delay="950" data-aos-duration="500">
          <router-link class="router-link" to="/add">
            <div>Ajouter un cours</div>
            <IonIcon name="add-circle"></IonIcon>
          </router-link>
        </div>
      </div>
    </div>
  </section>

</template>

<script>
import AOS from "aos";
import { IonIcon } from '@ionic/vue';
export default ({
  components: {
    IonIcon
  },
  data(){
    let search = '';

    return {
      search
    }
  },
  mounted() {
    AOS.init();
    this.$nextTick(() => {
      const screenHeight = window.innerHeight;
      document.querySelector('#sect1').style.height = screenHeight + "px";
    });
  }
})

</script>

<style lang="scss" scoped>
@import "../../../../utils/computer/components.scss";

.sect1-cours {
  position: relative;
  overflow: hidden;
  padding: 0px;

  &::before {
    position: absolute;
    content: '';
    height: 100%;
    width: 101%;
    bottom: -.5rem;
    background-image: url("../../../../assets/wave4.svg");
    background-repeat: no-repeat;
    background-position-y: 100%;
    left: 50%;
    transform: translateX(-50%);
  }

  .courses {
    position: absolute;
    left: 50%;
    top: 48%;
    transform: translate(-50%, -48%);
    width: 80%;

    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;

    .search-courses {
      position: relative;
      width: 45%;

      .input-search-courses {
        position: relative;
        width: 100%;
        margin-bottom: 2rem;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 1rem;

        input {
          position: relative;
          display: block;
          margin: 0;
          width: 70%;
          padding: 1.5rem;
          border-radius: $half-round 0 0 $half-round;

          outline: none;
          border: none;
          overflow: auto;
          box-sizing: border-box;
          box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
          background-color: $primary-white;
          z-index: 2;
          font-size: $h6-text;
          color: $primary-red;


          &:focus {
            border: none;
          }

          &::placeholder {
            color: $secondary-red;
          }
        }

        .div {
          border-radius: 0 $half-round $half-round 0;
          font-size: $h6-text;
          padding: 2.2rem;
          outline: none;
          display: flex;
          justify-content: center;
          align-items: center;
          height: 100%;
          border: none;
          box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
          background-color: $primary-white;
          z-index: 1;

          ion-icon {
            position: absolute;
            color: $secondary-red;
            font-size: $h5-text;
          }
        }
      }

    }


    .courses-shortcut {
      display: flex;
      align-items: center;
      justify-content: space-between;

      > div {
        margin-bottom: 2rem;
      }

      .all-courses {
        position: relative;
        display: flex;
        justify-content: center;
        margin-right: 1.5rem;

        .btn-cours {
          display: block;
          margin-right: 1.5rem;

          &:after {
            box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
          }
        }
      }
    }
  }
}

@media only screen and (max-width: 1024px) {

  // Taille d'écriture
  $small-text: 1.25rem;
  $p-text: 1.5rem;
  $big-text: 1.2rem;
  $h6-text: 1.3rem;
  $h5-text: 2.4rem;
  $h4-text: 2.9rem;
  $h3-text: 3.8rem;
  $h2-text: 4.9rem;
  $h1-text: 4rem;

  .sect1-cours {
    background-size: 250%;
    background-position-x: 20%;

    .courses {

      .search-courses {
        position: relative;
        width: 100%;

        .input-search-courses {
          position: relative;
          width: 100%;
          margin-bottom: 2rem;
          display: flex;
          justify-content: center;
          align-items: center;

          input {
            position: relative;
            display: block;
            margin: 0;
            width: 80%;
            padding: 1.5rem;
            border-radius: $half-round 0 0 $half-round;

            outline: none;
            border: none;
            box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
            background-color: $primary-white;

            z-index: 2;
            font-size: $h6-text;
            color: $primary-red;


            &:focus {
              border: none;
            }

            &::placeholder {
              color: $secondary-red;
            }
          }

          div {
            border-radius: 0 $half-round $half-round 0;
            font-size: $h6-text;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 2.6rem;
            outline: none;
            border: none;
            box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
            background-color: $primary-white;
            z-index: 1;

            ion-icon {
              position: absolute;
              color: $secondary-red;
              font-size: $h5-text;
            }
          }
        }

      }

      .courses-shortcut {
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-direction: column;

        > div {
          margin-bottom: 2rem;
        }

        .adding-course{
          display: none;
        }

        .all-courses {
          position: relative;
          width: 30%;
          display: flex;
          justify-content: center;

          .btn-cours {
            display: block;
            margin-bottom: 2rem;

            &:after {
              box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
            }

            .router-link {
              font-size: 1.2rem;
              padding: 0.8rem 1.2rem;
            }
          }
        }
      }


    }
  }
}

</style>