<template>
  <section id="sect7">
    <div class="admin-user-group">
      <h2>Utilisateurs</h2>
      <table>
        <thead>
        <tr>
          <th>Username</th>
          <th>Email</th>
          <th>Numéro étudiant</th>
          <th>Rôle</th>
          <th>Email_check</th>
          <th>Download</th>
          <th>Changement de permission</th>
          <th>Suppression</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(user, index) in userData" :key="index">
          <td>{{ user.username }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.student_number }}</td>
          <td>{{ user.role }}</td>
          <td>{{ user.is_checked }}</td>
          <td>{{ user.download_cout }}</td>
          <td>
            <label for="type">Rôle</label>
            <select name="type" v-model="user.role">
              <option value="Membre">Membre</option>
              <option value="Tuteur">Tuteur</option>
              <option value="Resp.Tuteur">Resp.Tuteur</option>
              <option value="Resp.Cours">Resp.Cours</option>
              <option value="Resp.Podcast">Resp.Podcast</option>
              <option value="Resp.Merch">Resp.Merch</option>
              <option value="Admin">Admin</option>
            </select>
          </td>
          <td>
            <div class="btn btn-primary btn-icon-forward btn-admin-choice" @click="deleteUSer(index)">
              <div>
                <IonIcon name="trash-outline"></IonIcon>
              </div>
            </div>
          </td>
        </tr>
        </tbody>
      </table>
      <div class="btn btn-primary btn-icon-forward btn-admin-choice" @click="validateChanges">
        <div>
          <div>Valider</div>
          <IonIcon name="checkmark-done-outline"></IonIcon>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import {IonIcon} from "@ionic/vue";

export default {
  components: {IonIcon},
  props: {
    adminUser: Boolean
  },
  data() {
    return {
      userData: [],
      originalRoles: [],
    }
  },
  methods: {
    deleteUSer (index) {
      const id_user = this.userData[index].id;
      axios.get(`https://delmoo.fr:5000/delete-user/${id_user}`, {
        withCredentials: true,
        validateStatus: function (status) {
          return status === 200 || status === 400 || status === 401 || status === 500;
        }
      }).then((res) => {
        if (res.status === 200) {
          window.location.reload();
        }
      })
    },
    roleToNumeric(role) {
      switch (role) {
        case "Membre":
          return 0;
        case "Tuteur":
          return 1;
        case "Resp.Tuteur":
          return 2;
        case "Resp.Cours":
          return 3;
        case "Resp.Podcast":
          return 4;
        case "Resp.Merch":
          return 5;
        case "Admin":
          return 6;
        default:
          return -1;
      }
    },
    updateSectionVisibility() {
      if (!this.adminUser) {
        document.getElementById("sect7").style.display = "none";
      } else {
        document.getElementById("sect7").style.display = "";
      }
    },
    validateChanges() {
      const indicesNonCorrespondants = this.originalRoles.map((roleOriginal, index) => {
        const roleActuel = this.userData[index].role;
        const roleOriginalNumerique = this.roleToNumeric(roleActuel);
        return roleActuel !== roleOriginal ? {index, nouveauRole: roleOriginalNumerique} : null;
      }).filter(information => information !== null);
      if (indicesNonCorrespondants.length === 0) {

        console.log("Les rôles correspondent pour chaque utilisateur.");
      } else {
        axios.post("https://delmoo.fr:5000/change-role", {
              idUser: indicesNonCorrespondants.map(info => this.userData[info.index].id),
              newRole: indicesNonCorrespondants.map(info => info.nouveauRole)
            },
            {
              withCredentials: true,
              validateStatus: function (status) {
                return status === 200 || status === 400 || status === 401 || status === 500;
              }
            }).then((res) => {
              if (res.status != 400) {
                window.location.reload();
              }
        })
      }
    }
  },
  mounted() {
    this.updateSectionVisibility();
    const screenHeight = window.innerHeight;
    document.querySelector('#sect7').style.height = screenHeight + "px";
    axios.get("https://delmoo.fr:5000/user", {
      withCredentials: true,
    }).then((result) => {
      this.userData = JSON.parse(JSON.stringify(result.data.json));
      this.userData.forEach(user => {
        switch (user.role) {
          case 0:
            user.role = "Membre";
            this.originalRoles.push("Membre");
            break;
          case 1:
            user.role = "Tuteur";
            this.originalRoles.push("Tuteur");
            break;
          case 2:
            user.role = "Resp.Tuteur";
            this.originalRoles.push("Resp.Tuteur");
            break;
          case 3:
            user.role = "Resp.Cours";
            this.originalRoles.push("Resp.Cours");
            break;
          case 4:
            user.role = "Resp.Podcast";
            this.originalRoles.push("Resp.Podcast");
            break;
          case 5:
            user.role = "Resp.Merch";
            this.originalRoles.push("Resp.Merch");
            break;
          case 6:
            user.role = "Admin";
            this.originalRoles.push("Admin");
            break;
        }
      });
    })
  },
  watch: {
    adminUser(newValue) {
      this.isAdminUser = newValue;
      this.updateSectionVisibility();
    },
    'userData': {
      handler(newVal) {
        newVal.forEach(user => user.originalRole = user.role);
      },
      deep: true,
    },
  },
}

</script>

<style lang="scss">
@import "../../../utils/computer/components.scss";

#sect7 {
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
    overflow-y: scroll;
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

        form, input, label {
          position: relative;
          display: block;
          border: none;
          outline: none;
        }
      }

      tbody tr {
        margin-bottom: 10px;
      }
    }
  }
}


</style>