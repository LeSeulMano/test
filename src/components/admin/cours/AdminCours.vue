<template>
  <section id="sect8">
    <div class="admin-user-group">
      <h2>Cours en cours de validation</h2>
      <table>
        <thead>
        <tr>
          <th>Nom</th>
          <th>Matière</th>
          <th>Promotion</th>
          <th>Type</th>
          <th>Prof</th>
          <th>Année</th>
          <th>Auteur</th>
          <th>Validé</th>
          <th>Refusé</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(cours, index) in coursData" :key="index">
          <td>
            <a :href="getFileURL(cours.path)" target="_blank">
              <label for="cours-name">{{ cours.name }}</label>
            </a>
            <input type="text" v-model="cours.name" name="cours-name"/>
          </td>
          <td>{{ cours.matter }}</td>
          <td>{{ cours.promotion }}</td>
          <td>{{ cours.type }}</td>
          <td>
            <label for="teacher-name">{{ cours.teacher }}</label>
            <input type="text" v-model="cours.teacher" name="teacher-name"/>
          </td>
          <td>{{ cours.year }}</td>
          <td>{{ cours.author }}</td>
          <td>
            <div class="btn pointer btn-cours"
                 @click="valide(cours.name, cours.teacher, cours.promotion, cours.type, cours.matter, cours.year, cours.author, cours.id)">
              <div class="valide">
                <IonIcon name="checkmark-outline"></IonIcon>
              </div>
            </div>
          </td>
          <td>
            <div class="btn pointer btn-cours">
              <div class="refuse">
                <IonIcon name="close-outline"></IonIcon>
              </div>
            </div>
          </td>
        </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>

<script>
import {IonIcon} from "@ionic/vue";
import axios from 'axios';

export default {
  components: {IonIcon},
  data() {
    return {
      coursData: [],
      updatedName: ''
    }
  },
  props: {
    adminUser: Boolean
  },
  watch: {
    adminUser(newValue) {
      this.isAdminUser = newValue;
      this.updateSectionVisibility();
    },
  },
  methods: {
    updateSectionVisibility() {
      if (!this.isAdminUser) {
        document.getElementById("sect8").style.display = "none";
      } else {
        document.getElementById("sect8").style.display = "block";
      }
    },
    getFileURL(coursPath) {
      return `https://delmoo.fr:5000/cours/open?file=${coursPath}`;
    },
    valide(name, teacher, promotion, type, matter, year, author, id) {
      axios.post(`https://delmoo.fr:5000/cours/valide`, {
            name: name,
            teacher: teacher,
            promotion: promotion,
            type: type,
            matter: matter,
            id: id
          },
          {
            withCredentials: true
          }).then(() => {
        window.location.reload();
      })
    }
  },
  mounted() {
    this.updateSectionVisibility();
    const screenHeight = window.innerHeight;
    document.querySelector('#sect8').style.height = screenHeight + "px";

    axios.get("https://delmoo.fr:5000/admin/cours").then((res) => {
      this.coursData = res.data.json;
    })

  },
}

</script>

<style lang="scss">
@import "../../../utils/computer/components.scss";

#sect8 {
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

  .admin-user-group {
    position: absolute;
    left: 50%;
    top: 60%;
    transform: translate(-50%, -50%);
    overflow: scroll;
    display: flex;
    align-items: center;
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

    table {
      border-collapse: collapse;
      width: 100%;

      th, td {
        text-align: left;
        padding: 2rem;

        input {
          background: rgba($color: $terciary-white, $alpha: .5); // rgba(35,35,35,0.5)
          -webkit-backdrop-filter: blur(10px);
          backdrop-filter: blur(10px);
          border: 1px solid rgba($color: $terciary-white, $alpha: .25); //rgba(35,35,35,0.25)

          // Effet d'ombre
          box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

          border-radius: $half-round;
          padding: 0.5rem;
          width: 100%;
        }

        form, input, label {

          position: relative;
          display: block;
          outline: none;
        }

        .btn {
          font-size: $h6-text;

          .valide:before {
            background-color: green;
          }

          .refuse:before {
            background-color: red;
          }
        }

        a {
          text-decoration: none;
          color: inherit;
        }
      }

      tbody tr {
        margin-bottom: 10px;
      }
    }
  }

}


</style>