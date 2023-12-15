<template>
  <header id="admin-header">
    <nav>
      <div class="btn pointer btn-icon-forward btn-admin-choice" :class="selected == 0 ? 'btn-secondary' : 'btn-primary'" @click="selectPage(0)">
        <div>
          <div>Users</div>
          <IonIcon name="person-outline"></IonIcon>
        </div>
      </div>
      <div class="btn pointer btn-icon-forward btn-admin-choice" :class="selected == 1 ? 'btn-secondary' : 'btn-primary'" @click="selectPage(1)">
        <div>
          <div>Cours</div>
          <IonIcon name="document-outline"></IonIcon>
        </div>
      </div>
      <div class="btn pointer btn-icon-forward btn-admin-choice" :class="selected == 2 ? 'btn-secondary' : 'btn-primary'" @click="selectPage(2)">
        <div>
          <div>Podcast</div>
          <IonIcon name="mic-outline"></IonIcon>
        </div>
      </div>
      <div class="btn pointer btn-icon-forward btn-admin-choice" :class="selected == 3 ? 'btn-secondary' : 'btn-primary'" @click="selectPage(3)">
        <div>
          <div>Merch</div>
          <IonIcon name="cut-outline"></IonIcon>
        </div>
      </div>
      <div class="btn pointer btn-icon-forward btn-admin-choice" :class="selected == 4 ? 'btn-secondary' : 'btn-primary'" @click="selectPage(4)">
        <div>
          <div>Tutorat</div>
          <IonIcon name="pencil-outline"></IonIcon>
        </div>
      </div>
      <div class="btn pointer btn-primary btn-icon-forward btn-admin-choice" :class="selected == 5 ? 'btn-secondary' : 'btn-primary'" @click="selectPage(5)">
        <div>
          <div>Manage</div>
          <IonIcon name="desktop-outline"></IonIcon>
        </div>
      </div>
    </nav>
  </header>
</template>

<script>
import {IonIcon} from "@ionic/vue";
export default {
  components: {IonIcon},
  props: {
    selected: Number
  },
  methods: {
    selectPage(index) {
      this.$emit('selectPage', index);
    }
  }
}
</script>

<style lang="scss">
@import "../../../utils/computer/variables.scss";

#admin-header{
  top: 11.5%;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
  height: 7rem;
  width: 100%;
  background-color: $terciary-white;
  transition: .3s;

  .btn-admin-choice{
    margin-right: 1.5rem;
  }
}

</style>