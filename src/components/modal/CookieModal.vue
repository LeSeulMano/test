<template>
  <div class="cookie-content" v-if="!accepted">
      <h2>Nous nous soucions de votre vie privée</h2>
      <p>Nous utilisons des cookies sur notre site pour faciliter votre expérience, pour cela il faut les accepter. Comme de toute facon personne ne lit
      ce genre de message je peux vous dire que ces cookies sont là pour faire des stats et les stats c'est fun ! Tous le monde clique sur refusé à chaque fois
      parce que askip les cookies c'est mal, mais bon nous on a rien comme cookie donc si tu lis ce message accepte stp. Si tu veux voir notre politique de confidentialité
        clique
      là dessus, de toute facon tu liras pas mais bon on est obligé...</p>
    <div class="btn-action">
      <div class="btn pointer btn-primary btn-icon" @click="acceptCookies">
        <div>
          <div>Accepter</div>
        </div>
      </div>
      <div class="btn pointer btn-secondary btn-icon" @click="refuseCookies">
        <div>
          <div>Refuser</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      accepted: this.$cookies.get('cookiesAccepted') === 'true',
    };
  },
  methods: {
    acceptCookies() {
      this.$cookies.set('cookiesAccepted', 'true');
      this.accepted = true;
      this.$emit('accept-cookies');
    },
    refuseCookies() {
      this.$cookies.set('cookiesAccepted', 'false');
      this.accepted = true;
    },
  },
}

</script>

<style lang="scss">
@import '../../utils/computer/components.scss';
.cookie-content{
  background: rgba($color: $terciary-white, $alpha: .5); // rgba(35,35,35,0.5)
  -webkit-backdrop-filter: blur(10px);
  backdrop-filter: blur(10px);
  border: 1px solid rgba($color: $terciary-white, $alpha: .25); //rgba(35,35,35,0.25)

  // Effet d'ombre
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

  border-radius: $half-round 0;
  padding: 1rem 3rem 1rem 3rem;
  position: fixed;
  bottom: 0;
  z-index: 1000;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  p{
    padding-right: 4em;
  }
  .btn-action{
    display: flex;
    .btn{
      margin-right: 2rem;
    }
  }

}

</style>