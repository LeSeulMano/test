<template>
  <headerComp :selected="5"></headerComp>
  <main>
    <section id="sect6">
      <div class="condition-general">
        <h3>Conditions générales d'utilisation</h3>
        <div class="content">
          <p> En utilisant ce site web, vous acceptez les conditions suivantes. Veuillez les lire attentivement avant de
            poursuivre. </p>
          <h6> 1. Acceptation des termes </h6>
          <p>En cochant la case d’acceptation des conditions d’utilisation générales du site web “delmoo.fr” au moment
            de la création de votre compte, vous acceptez de vous conformer à ces mêmes conditions générales
            d'utilisation, et à toutes les lois et réglementations applicables. Si vous n’acceptez pas ces conditions
            d’utilisation, veuillez ne pas utiliser le site.</p>
          <h6>2. Utilisation du site</h6>
          <p>Vous acceptez d’utiliser ce site uniquement à des fins légales et conformément à ces conditions. Vous vous
            engagez à ne pas utiliser le site de manière à enfreindre les droits d’autrui, à nuire à d’autres
            utilisateurs ou à compromettre la sécurité du site.
          </p>
          <h6>3. Création du compte</h6>
          <p>Certaines fonctionnalités du site peuvent nécessiter la création d'un compte. Vous êtes responsable de
            maintenir la confidentialité de vos informations de compte et de toutes les activités qui se produisent sous
            votre compte.</p>
          <h6>4. Propriété Intellectuelle</h6>
          <p>Tous les contenus présents sur ce site, y compris les textes, images, logos, sont la propriété de DELMOO et
            sont protégés par les lois sur la propriété intellectuelle. Vous ne pouvez pas reproduire, distribuer ou
            utiliser ces contenus sans notre autorisation écrite.</p>
          <h6> 5. Comportement de l’utilisateur</h6>
          <p>En utilisant ce site, vous acceptez de vous abstenir de tout comportement qui pourrait porter atteinte à
            d'autres utilisateurs, nuire au bon fonctionnement du site ou enfreindre les lois en vigueur. Il est
            interdit de diffuser des contenus illégaux, offensants, diffamatoires, ou de s'engager dans des activités
            perturbatrices. Nous nous réservons le droit de prendre des mesures, y compris la résiliation du compte, en
            cas de violation de cette disposition.
          </p>
          <h6>6. Résiliation du compte
          </h6>
          <p>Nous nous réservons le droit de résilier votre compte et de refuser l'accès au site pour quelque raison que
            ce soit, y compris, sans limitation, si nous estimons que vous avez enfreint ces Conditions d'Utilisation.
            La résiliation peut se faire sans préavis. Vous pouvez également résilier votre compte à tout moment en en
            faisant la demande à notre adresse mail suivante : delmoo.pro@gmail.com</p>
          <h6>7. Modifications des conditions</h6>
          <p>Nous nous réservons le droit de modifier unilatéralement ces Conditions Générales d'Utilisation à tout
            moment. Les modifications prendront effet dès leur publication sur le site, et il vous incombe de consulter
            régulièrement les conditions mises à jour. En continuant à utiliser le site après la publication des
            modifications, vous acceptez les nouvelles conditions.
          </p>
          <br>
          <p>Merci de votre compréhension et de votre collaboration !<br>L’équipe DELMOO
          </p>
          <br>
          <span>Date de la dernière mise à jour : 11/12/2023</span>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import headerComp from "@/components/header/HeaderComp.vue";

export default ({
  components: {headerComp},
  mounted() {
    const screenHeight = window.innerHeight;
    document.querySelector('#sect6').style.height = screenHeight + "px";
  }
})
</script>

<style lang="scss">
@import "../../utils/computer/variables.scss";

main {
  position: relative;

  #sect6 {
    position: relative;
    overflow: hidden;
    padding: 0px;
    display: flex;

    &::before {
      position: absolute;
      content: '';
      height: 100%;
      width: 101%;
      bottom: -.5rem;
      background-image: url("../../assets/wave1.svg");
      background-repeat: no-repeat;
      background-position-y: 100%;
      left: 50%;
      transform: translateX(-50%);
    }

    .condition-general {
      position: absolute;
      left: 50%;
      top: 20%;
      transform: translateX(-50%);
      overflow-y: scroll;
      max-height: 40rem;
      // Effet Glass Morphism
      background: rgba($color: $terciary-white, $alpha: .5); // rgba(35,35,35,0.5)
      -webkit-backdrop-filter: blur(10px);
      backdrop-filter: blur(10px);
      border: 1px solid rgba($color: $terciary-white, $alpha: .25); //rgba(35,35,35,0.25)

      // Effet d'ombre
      box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

      border-radius: $half-round;
      padding: 2.2rem 3rem 2rem 3rem;

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

      .content {
        h6 {
          margin-top: 2rem;
          font-size: $h6-text;
        }
      }

      h3 {
        text-align: center;
        margin: 0;
        margin-bottom: 2.5rem;
        font-size: $h3-text;
      }
    }
  }
}

@media only screen and (max-width: 1024px) {
  $h6-text: 1.2rem;
  $h3-text: 3.8rem;
  $h5-text: 2.4rem;
  $h4-text: 2.9rem;
  main {
    #sect6 {
      .condition-general {
        max-height: 30rem;

        h3 {
          font-size: $h4-text;
        }
      }
    }
  }
}

</style>