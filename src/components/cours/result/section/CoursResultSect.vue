<template>
  <section id="sect1" class="sect-cours-result">

    <div class="courses">
      <div class="courses-header">
        <div class="style-bar"></div>
        <div class="search-bar">
          <input type="text" placeholder="Quel cours recherches-tu ?" v-model="search">
          <div @click="reload">
            <IonIcon name="search"></IonIcon>
          </div>
        </div>
        <div class="style-bar"></div>
      </div>
      <div class="courses-body">
        <div class="subject-courses">
          <div class="subject-school-year">
            <div class="btn btn-cours btn-no-hover pointer" :class="currentYear == 1 ? 'btn-school-year-selected' : ''"
                 @click="onYearChange(1)">
              <div>
                <div>1A</div>
              </div>
            </div>
            <div class="btn btn-cours btn-no-hover pointer" :class="currentYear == 2 ? 'btn-school-year-selected' : ''"
                 @click="onYearChange(2)">
              <div>
                <div>2A</div>
              </div>
            </div>
            <div class="btn btn-cours btn-no-hover pointer"
                 :class="(currentYear == 3 || currentSpe != 0) ? 'btn-school-year-selected' : ''"
                 @click="onYearChange(3)">
              <div>
                <div>3A</div>
              </div>
            </div>
          </div>
          <div class="subject-courses-btn">
            <div class="btn btn-primary pointer btn-no-hover" v-for="(matter, index) in currentMatterYear" :key="index"
                 @click="onMatterChange($event)">
              <div class="matter" :data-tooltip="currentMatterYearTooltip[index]">
                <div>{{ matter }}</div>
              </div>
            </div>
          </div>
          <div class="subject-courses-select">
            <select>
              <option v-for="(matter, index) in currentMatterYear" :key="index" :value="matter">{{ matter }}</option>
            </select>
          </div>
        </div>
        <div class="file-courses">
          <div class="file-type-filter">
            <div class="btn btn-no-hover file-type-cm pointer btn-cours"
                 :class="currentType == 1 ? 'btn-filter-cm-selecter' : ''" @click="currentType = 1">
              <div>
                <div>CM</div>
              </div>
            </div>
            <div class="btn btn-no-hover file-type-td pointer btn-shop"
                 :class="currentType == 2 ? 'btn-filter-td-selecter' : ''" @click="currentType = 2">
              <div>
                <div>TD</div>
              </div>
            </div>
            <div class="btn btn-no-hover file-type-fiche pointer btn-podcast"
                 :class="currentType == 3 ? 'btn-filter-fiche-selecter' : ''" @click="currentType = 3">
              <div>
                <div>Fiches</div>
              </div>
            </div>
            <div v-if="(currentYear === 3 || currentSpe != 0)" class="spe-third">
              <div class="btn pointer btn-cours" :class="currentSpe == 1 ? 'btn-filter-cm-selecter' : ''"
                   @click="onYearChange(4)">
                <div>
                  <div>ME</div>
                </div>
              </div>
              <div class="btn pointer btn-cours" :class="currentSpe == 2 ? 'btn-filter-cm-selecter' : ''"
                   @click="onYearChange(5)">
                <div>
                  <div>MT</div>
                </div>
              </div>
              <div class="btn pointer btn-cours" :class="currentSpe == 3 ? 'btn-filter-cm-selecter' : ''"
                   @click="onYearChange(6)">
                <div>
                  <div>IIA</div>
                </div>
              </div>
              <div class="btn pointer btn-cours" :class="currentSpe == 4 ? 'btn-filter-cm-selecter' : ''"
                   @click="onYearChange(7)">
                <div>
                  <div>GCB</div>
                </div>
              </div>
              <div class="btn pointer btn-cours" :class="currentSpe == 5 ? 'btn-filter-cm-selecter' : ''"
                   @click="onYearChange(8)">
                <div>
                  <div>GI</div>
                </div>
              </div>
            </div>
          </div>
          <div v-if="currentYear === 3" class="subject-spe-select">
            <select>
              <option value="ME">ME</option>
              <option value="MT">MT</option>
              <option value="IIA">IIA</option>
              <option value="GCB">GCB</option>
              <option value="GI">GI</option>
            </select>
          </div>
          <div class="files">
            <CoursCard
                v-for="(cour, index) in cours.filter(c => c.type == (currentType == 1 ? 'CM' : currentType == 2 ? 'TD' : currentType == 3 ? 'Fiche' : 'CM') && c.matter == currentMatter && c.promotion == currentYear)"
                :key="index" :matter="cour.matter" :type="cour.type" :date="cour.year" :name="cour.name"
                :path="cour.path"></CoursCard>
          </div>
        </div>
      </div>
    </div>
    <ErrorModal :errorModalVisible="errorModalVisible" :message="errorMessage"
                @update:errorModalVisible="updateErrorModal"></ErrorModal>
  </section>
</template>

<script>
import {IonIcon} from '@ionic/vue';
import CoursCard from "@/components/cours/result/card/CoursCard.vue";
import axios from 'axios';
import ErrorModal from "@/components/modal/ErrorModal.vue";
import anime from "animejs";

export default ({
  components: {
    CoursCard,
    IonIcon,
    ErrorModal
  },
  data() {
    const first = ["Analyse", "Algèbre", "OMP", "Algo", "Electrostat", "Electrocin", "Mécanique", "Thermo", "CDM", "Proba", "Magnétostatique", "Optique", "Auto"];
    const second = ["Analyse", "Algèbre", "Algo", "BDR", "Architecture", "Electromag", "PDO", "OptiqueOndulatoire", "Auto", "ANUM", "Thermo", "CalculFormel", "POO", "GL"];
    const mt = ["TDS", "C/python", "Electronique", "Electrotechnique", "Automatique", "Analyse", "RDM", "Mécanique", "Procédé", "Modélisation"];
    const iia = ["Méca","MPES","Electrotechnique","Signaux","AnalyseSys","CalculEmbarqué"]
    const gi = ["TDS", "C/python", "Electronique", "Electrotechnique", "Automatique", "Analyse", "RDM", "Proba", "Procédé", "Modélisation", "Maintenance"];
    const gcb = ["Plan", "CAO", "GrosOeuvre", "SecondOeuvre", "Economie", "Architecture", "MecaniqueSol", "RDM"];
    const me = ["AA", "RDM", "Fab", "Capteurs", "Electrotech", "Mecaflu", "TSM", "Conception", "Meca", "Proba", "Thermo", "MMC", "ANUM", "Energétique"];

    const firstTooltip = ["Analyse", "Algèbre", "Outil Mathématique", "Algo", "Electrostatique", "Electrocinétique", "Mécanique", "Thermodynamique", "Chimie Des Matériaux", "Probabilité", "Magnétostatique", "Optique Géométrique", "Automatique"];
    const secondTooltip = ["Analyse", "Algèbre", "Algo", "Base de Donnée Relationnelle", "Architecture", "Electromagnétisme", "Physique des Ones", "Optique Ondulatoire", "Automatique", "Analyse Numérique", "Thermodynamique", "Calcul Formel", "Programmation Orienté Objet", "Génie Logiciel"];
    const mtTooltip = ["Traitement de Signal", "C/python", "Electronique", "Electrotechnique", "Automatique", "Analyse", "Résistance des Matériaux", "Mécanique", "Procédé de Fabrication", "Modélisation"];
    const iiaTooltip = ["Mécanique","Méthode pilotage systèmes discrets","Electrotechnique","Signaux","Analyse Sytèmes","CalculEmbarqué"]
    const giTootlip = ["Traitement de Signal", "C/python", "Electronique", "Electrotechnique", "Automatique", "Analyse", "Résistance des Matériaux", "Probabilité", "Procédé de Fabrication", "Modélisation", "Maintenance"];
    const gcbTooltip = ["Plan", "CAO", "GrosOeuvre", "SecondOeuvre", "Economie de la Maitrise de l'oeuvre", "Architecture", "Mecanique des Sols", "Résistance des Matériaux"];
    const meTooltip = ["Analyse Appliqué", "Résistance des Matériaux", "Procédé de Fabrication", "Capteurs pour la mécanique", "Electrotechnique", "Mécanique des fluides", "Traitement de signal en Mécanique", "Conception", "Mecanique", "Probabilité", "Thermodynamique", "Mécanique des milieu continus", "Analyse Numérique", "Energétique"];


    let currentYear = 1;
    let currentSpe = 0;
    let currentType = 1;
    let currentMatter = currentYear == 1 ? first[0] : currentYear == 2 ? second[0] : currentSpe == 1 ? me[0] : currentSpe == 2 ? mt[0] : currentSpe == 3 ? iia[0] : currentSpe == 4 ? gcb[0] : currentSpe == 5 ? gi[0] : first[0];
    let search = '';
    let cours = [];
    const link = 'https://delmoo.fr:5000';
    return {
      firstTooltip,
      secondTooltip,
      meTooltip,
      mtTooltip,
      iiaTooltip,
      gcbTooltip,
      giTootlip,
      errorModalVisible: false,
      errorMessage: "",
      currentMatterYear: first,
      currentMatterYearTooltip: firstTooltip,
      first,
      second,
      currentYear,
      currentType,
      search,
      cours,
      link,
      currentMatter,
      currentSpe,
      mt,
      me,
      iia,
      gcb,
      gi,
    }
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
    reload() {
      this.$router.push({name: 'Result', query: {search: this.search}}).then(() => {
        window.location.reload()
      })
    },
    onMatterChange(event) {
      document.querySelectorAll(".btn-subject-selected")[0].classList.remove("btn-subject-selected");
      event.currentTarget.classList.add('btn-subject-selected');
      this.currentMatter = event.currentTarget.lastChild.innerText;
    },
    onYearChange(year) {
      document.querySelectorAll(".btn-subject-selected")[0].classList.remove("btn-subject-selected");
      document.querySelectorAll(".pointer")[3].classList.add("btn-subject-selected");
      this.currentMatter = year == 1 ? this.first[0] : year == 2 ? this.second[0] : year == 3 ? this.me[0] : year == 4 ? this.me[0] : year == 5 ? this.mt[0] : year == 6 ? this.iia[0] : year == 7 ? this.gcb[0] : year == 8 ? this.gi[0] : this.first[0];


      if (year == 3) {
        this.currentSpe = 1;
        this.currentYear = 3;
      }
      if (year != 1 && year != 2 && year != 3) {
        this.currentSpe = year - 3;
      }
      if (year == 1 || year == 2) {
        this.currentSpe = 0;
        this.currentYear = year;
      }
      this.currentMatterYear = year == 1 ? this.first : year == 2 ? this.second : year == 3 ? this.me : year == 4 ? this.me : year == 5 ? this.mt : year == 6 ? this.iia : year == 7 ? this.gcb : year == 8 ? this.gi : this.first;
      this.currentMatterYearTooltip = year == 1 ? this.firstTooltip : year == 2 ? this.secondTooltip : year == 3 ? this.meTooltip : year == 4 ? this.meTooltip : year == 5 ? this.mtTooltip : year == 6 ? this.iiaTooltip : year == 7 ? this.gcbTooltip : year == 8 ? this.giTootlip : this.firstTooltip;

    }
  },

  mounted() {
    const header = document.querySelector('header');
    const screenHeight = window.innerHeight;
    let section1 = document.getElementById('sect1')
    let headerHeight = header.offsetHeight
    section1.style.paddingTop = headerHeight + 'px'
    document.querySelector('#sect1').style.height = screenHeight - headerHeight + "px";
    if (window.matchMedia("(min-width: 1024px)").matches) {
      document.addEventListener("scroll", () => {
        if (document.documentElement.scrollTop > 5 && !document.getElementById('menu-opener').classList.contains('opened')) {
          header.classList.add('header-hidden')
        } else if (!document.getElementById('menu-opener').classList.contains('opened')) {
          header.classList.remove("header-hidden");
        }
      });
    }

    const search = this.$route.query.search;
    let isYear = false;
    switch (search) {
      case "1":
        axios.post(this.link + "/cours/year", {
          year: '1'
        }, {
          validateStatus: function (status) {
            return status === 201 || status === 500;
          }
        }).then((res) => {
          if (res.status == 500) {
            this.showErrorModal(res.data.message);
            return;
          }
          this.cours = res.data.json;
        })
        this.currentYear = 1;
        isYear = true;
        break;
      case '2':
        axios.post(this.link + "/cours/year", {
          year: '2'
        }, {
          validateStatus: function (status) {
            return status === 201 || status === 500;
          }
        }).then((res) => {
          if (res.status == 500) {
            this.showErrorModal(res.data.message);
            return;
          }
          this.cours = res.data.json;
        })
        isYear = true;
        this.currentYear = 2;
        break;
      case '3':
        axios.post(this.link + "/cours/year", {
          year: '3'
        }, {
          validateStatus: function (status) {
            return status === 201 || status === 500;
          }
        }).then((res) => {
          if (res.status == 500) {
            this.showErrorModal(res.data.message);
            return;
          }
          this.cours = res.data.json;
        })
        isYear = true;
        this.currentYear = 3;
        break;
    }
    if (!search) {
      axios.get(this.link + "/cours", {
        validateStatus: function (status) {
          return status === 201 || status === 500;
        }
      }).then((res) => {
        if (res.status == 500) {
          this.showErrorModal(res.data.message);
          return;
        }
        this.cours = res.data.json;
      })
    } else {
      if (!isYear) {
        axios.post(this.link + "/cours/search", {
          search: search
        }, {
          validateStatus: function (status) {
            return status === 201 || status === 500;
          }
        }).then((res) => {
          if (res.status == 500) {
            this.showErrorModal(res.data.message);
            return;
          }
          this.cours = res.data.json;
        })
      }
    }
    document.querySelectorAll(".pointer")[3].classList.add("btn-subject-selected");
  }
})
</script>

<style lang="scss">
@import "../../../../utils/computer/components.scss";

.sect-cours-result {
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
    background-image: url("../../../../assets/wave4.svg");
    background-repeat: no-repeat;
    background-position-y: 100%;
    left: 50%;
    transform: translateX(-50%);
  }

  .courses {
    position: relative;
    margin: 0 auto;
    margin-top: 2rem;
    width: 80%;
    padding: 1.9rem;
    border-radius: $half-round;

    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;

    // Effet Glass Morphism
    background: rgba($color: $primary-black, $alpha: .5); // rgba(35,35,35,0.5)
    -webkit-backdrop-filter: blur(10px);
    backdrop-filter: blur(10px);
    border: 1px solid rgba($color: $primary-black, $alpha: .25); //rgba(35,35,35,0.25)

    // Effet d'ombre
    box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

    .courses-header {
      position: relative;
      display: flex;
      width: 100%;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 2.2rem;


      .style-bar {
        width: 32%;
        height: 5px;
        transform: skewX(-20deg);
        background-color: $primary-red;
      }

      .search-bar {
        display: flex;
        align-items: center;
        width: 30%;
        margin: 0 .5rem;

        input {
          margin: 0;
          display: flex;
          justify-content: center;
          align-items: center;
          border-radius: $half-round 0 0 $half-round;
          padding: 15px 25%;
          outline: none;
          border: none;
          box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
          background-color: $primary-white;

          z-index: 2;
          font-size: $p-text;
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
          padding: 25px 1.6rem;
          cursor: pointer;
          box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
          background-color: $primary-white;
          z-index: 1;
          display: flex;
          height: 100%;
          justify-content: center;
          align-items: center;

          ion-icon {
            position: absolute;
            color: $secondary-red;
            font-size: $big-text;
          }
        }
      }

    }

    .courses-body {

      position: relative;
      display: flex;
      justify-content: space-between;
      height: 100%;
      width: 100%;

      .subject-courses {
        position: relative;
        height: 90%;
        padding-left: .5rem;
        width: 16rem;
        margin-right: 5rem;

        .subject-courses-select {
          display: none;
        }

        .subject-school-year {
          position: relative;
          display: flex;
          padding-left: .5rem;
          margin-right: 2rem;
          margin-bottom: 1.5rem;

          .btn {
            &:not(.btn:last-of-type) {
              margin-right: 1rem;
            }

            > div {
              padding: .6rem .8rem;

              > div {
                font-size: $p-text;
                color: $primary-white;
                font-weight: 600;
                text-transform: uppercase;
              }
            }
          }

          .btn-school-year-selected {
            > div::before {
              background-color: $primary-white;
            }

            > div > div {
              color: $cours-color;
            }
          }

        }

        .subject-courses-btn {

          position: relative;
          overflow-y: scroll;
          overflow-x: hidden;
          padding-right: 1rem;
          padding-left: .4rem;
          width: 100%;
          max-height: 59vh;
          z-index: 5;

          .matter{
            position: relative;
          }

          .matter::after{
            content: attr(data-tooltip);
            position: absolute;
            left: 50%;
            bottom: -30px;
            transform: translateX(-50%);
            background-color: rgba(249, 243, 241, 0.5);
            box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
            backdrop-filter: blur(10px);
            color: rgb(75, 75, 75);
            font-weight: bold;
            padding: 5px;
            font-size: 1rem;
            border-radius: 5px;
            opacity: 0;
            transition: opacity 0.3s;
            white-space: nowrap;
          }

          .matter:hover::after{
            opacity: 1;
          }

          .btn {
            display: block;
            font-size: $h6-text;
            color: $primary-white;
            font-weight: 600;
            width: 14rem;
            box-shadow: none;

            > div::before {
              box-shadow: none;
            }

            &.btn-subject-selected {
              > div:before {
                background-color: $primary-white;
              }

              > div > div {
                color: $primary-red;
              }
            }

            &:not(.btn:last-of-type) {
              margin-bottom: 1.7rem;
            }
          }

        }

      }

      .file-courses {
        position: relative;
        width: 100%;

        .subject-spe-select {
          display: none;
        }

        .file-type-filter {
          display: flex;

          .spe-third {
            display: flex;
            position: relative;
          }

          .btn {
            &:not(.btn:last-of-type) {
              margin-right: .85rem;
            }

            > div {
              padding: .55rem .9rem;

              > div {
                font-size: $p-text;
                color: $primary-white;
                font-weight: 600;
                text-transform: uppercase;
              }
            }

            &.btn-filter-cm-selecter {
              > div::before {
                background-color: $primary-white;
              }

              > div > div {
                color: $cours-color;
              }
            }

            &.btn-filter-td-selecter {
              > div::before {
                background-color: $primary-white;
              }

              > div > div {
                color: $shop-color;
              }
            }

            &.btn-filter-fiche-selecter {
              > div::before {
                background-color: $primary-white;
              }

              > div > div {
                color: $podcast-color;
              }
            }
          }
        }


        .files {
          position: relative;
          display: flex;
          flex-wrap: wrap;
          margin-top: 1.5rem;
          overflow-y: scroll;
          max-height: 59vh;
        }

      }


    }
  }
}

@media only screen and (max-width: 1024px) {

  // Taille d'écriture
  $small-text: 1.25rem;
  $p-text: 1rem;
  $big-text: 1.2rem;
  $h6-text: 1.2rem;
  $h5-text: 2.4rem;
  $h4-text: 2.9rem;
  $h3-text: 3.8rem;
  $h2-text: 4.9rem;
  $h1-text: 6.4rem;

  .sect-cours-result {
    padding-bottom: 3rem;

    .courses {
      margin-top: 2rem;
      padding: 1.9rem;
      flex-wrap: wrap;

      .courses-header {
        position: relative;
        display: flex;
        width: 100%;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 2.5rem;

        .style-bar {
          width: 0%;
          height: 5px;
          transform: skewX(-20deg);
          background-color: $primary-red;
        }

        .search-bar {
          width: 100%;
          margin: 0 0rem;

          input {
            padding: 1.1rem 1.55rem;
            font-size: $p-text;
          }

          div {
            padding: 1.9rem 1.5rem;

            ion-icon {
              font-size: $h6-text;
            }
          }
        }

      }

      .courses-body {
        flex-wrap: wrap;

        .subject-courses {
          position: relative;
          height: 90%;
          padding-left: 0;
          width: 100%;
          margin-right: 0;
          margin-bottom: .2rem;

          .subject-school-year {
            padding-left: .5rem;
            margin-right: 0;
            margin-bottom: 1.5rem;

            .btn {
              &:not(.btn:last-of-type) {
                margin-right: 1.2rem;
              }

              > div {
                padding: .8rem 1rem;

                > div {
                  font-size: $p-text;
                }
              }
            }

            .btn-school-year-selected {
              > div::before {
                background-color: $primary-white;
              }

              > div > div {
                color: $cours-color;
              }
            }

          }

          .subject-courses-btn {
            display: none;
          }

          .subject-courses-select {
            display: block;
            position: relative;

            &:before {
              position: absolute;
              content: url("../../../../assets/chevron-down.svg");
              left: 96%;
              top: 50%;
              transform: translate(-96%, -50%);
              width: 1.3rem;
              border-radius: $half-round;
              z-index: 5;
            }

            select {
              position: relative;
              display: block;
              margin: 0;
              margin-bottom: 2.5rem;
              width: 100%;
              padding: 1.2rem 1.55rem;
              border-radius: $half-round;

              outline: none;
              border: none;
              box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
              background-color: $primary-red;

              z-index: 2;
              font-size: $p-text;
              color: $primary-white;
              font-weight: 600;

              &:focus {
                border: none;
              }

              &::placeholder {
                color: $secondary-red;
              }

              // Supprimer la fleche pour ouvrir par défaut
              &::-ms-expand {
                display: none;
              }

              appearance: none;
              -webkit-appearance: none;
              -moz-appearance: none;

              option {
                font-size: $p-text;
                color: $primary-white;
                font-weight: 600;
                min-height: 0;
                padding: .6rem;
              }
            }
          }

        }

        .file-courses {
          position: relative;
          width: 100%;

          .subject-spe-select {
            display: block;
            position: relative;

            &:before {
              position: absolute;
              content: url("../../../../assets/chevron-down.svg");
              left: 96%;
              top: 50%;
              transform: translate(-96%, -50%);
              width: 1.3rem;
              border-radius: $half-round;
              z-index: 5;
            }

            select {
              position: relative;
              display: block;
              margin: 0;
              margin-bottom: 2.5rem;
              width: 100%;
              padding: 1.2rem 1.55rem;
              border-radius: $half-round;

              outline: none;
              border: none;
              box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
              background-color: $primary-red;

              z-index: 2;
              font-size: $p-text;
              color: $primary-white;
              font-weight: 600;

              &:focus {
                border: none;
              }

              &::placeholder {
                color: $secondary-red;
              }

              // Supprimer la fleche pour ouvrir par défaut
              &::-ms-expand {
                display: none;
              }

              appearance: none;
              -webkit-appearance: none;
              -moz-appearance: none;

              option {
                font-size: $p-text;
                color: $primary-white;
                font-weight: 600;
                min-height: 0;
                padding: .6rem;
              }
            }
          }

          .file-type-filter {
            padding-left: .5rem;
            padding-bottom: 1.5rem;

            .spe-third {
              display: none;
            }

            .btn {
              &:not(.btn:last-of-type) {
                margin-right: .85rem;
              }

              > div {
                padding: .75rem 1.1rem;

                > div {
                  font-size: $p-text;
                  color: $primary-white;
                  font-weight: 600;
                  text-transform: uppercase;
                }
              }

              &.btn-filter-cm-selecter {
                > div::before {
                  background-color: $primary-white;
                }

                > div > div {
                  color: $cours-color;
                }
              }

              &.btn-filter-td-selecter {
                > div::before {
                  background-color: $primary-white;
                }

                > div > div {
                  color: $shop-color;
                }
              }

              &.btn-filter-tp-selecter {
                > div::before {
                  background-color: $primary-white;
                }

                > div > div {
                  color: $podcast-color;
                }
              }
            }
          }

          .files {
            margin-top: 1.5rem;
            max-height: 59vh;

            .file {
              position: relative;
              //margin-right: 3%;
              margin-bottom: 3%;
              padding: 1.5rem;
              border-radius: $half-round;
              width: 10%;
              min-width: 290px;

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
          }

          .file-type-cm div::before {
            background-color: $cours-color;
          }

          .file-type-td div::before {
            background-color: $shop-color;
          }

          .file-type-tp div::before {
            background-color: $podcast-color;
          }

        }
      }
    }

  }

}
</style>