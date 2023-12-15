<template>
  <div class="team-member" data-aos="fade-down" data-aos-duration="600">
    <div class="team-member-img"><img id="image-staff" :src="url != 'team4' ? getImageUrl() : ''"></div>
    <h3>{{ name }}</h3>
    <h4>{{ role }}</h4>
  </div>
</template>

<script>
export default {
  name: 'StaffMembers',
  props: {
    role: String,
    name: String,
    url: String
  },
  methods: {
    getImageUrl() {
      return require(`../../../../assets/${this.url}`);
    }
  }
}

</script>

<style lang="scss">
@import '../../../../utils/computer/variables';

.team-members {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  margin-top: 3.5rem;


  .team-member {
    position: relative;
    border-radius: $half-round;
    width: 20%;
    max-width: 200px;
    padding: 2rem 0;

    // Effet Glass Morphism
    background: rgba($color: $terciary-white, $alpha: .5); // rgba(35,35,35,0.5)
    -webkit-backdrop-filter: blur(10px);
    backdrop-filter: blur(10px);
    border: 1px solid rgba($color: $terciary-white, $alpha: .25); //rgba(35,35,35,0.25)

    // Effet d'ombre
    box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

    &:not(.team-member:last-of-type) {
      margin-right: 5%;
    }

    // Ligne sur le côté
    &:after, &:before {
      position: absolute;
      content: '';
      top: .5rem;
      width: 4px;
      background-color: $primary-red;
      transform: skewY(30deg);
    }

    &:after {
      left: -.8rem;
      height: 6rem;
    }

    &:before {
      left: -1.4rem;
      height: 3.7rem;
    }

    .team-member-img {
      position: relative;
      margin: 0 auto;
      width: 7.1vw;
      height: 7.1vw;
      border-radius: $round;
      overflow: hidden;
      margin-bottom: 1.2rem;
      border: 3px solid $secondary-red;

      img {
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        height: 100%;
      }
    }

    h3, h4 {
      text-align: center;
    }

    h3 {
      position: relative;
      font-size: $h5-text;
      //color: $primary-red;
      margin: 0;

      &:after {
        position: absolute;
        content: '';
        bottom: -.55rem;
        left: 50%;
        transform: translateX(-50%);
        background-color: $secondary-red;
        width: 4rem;
        height: 2px;
        font-weight: 700;
      }
    }

    h4 {
      font-size: $p-text;
      color: $secondary-red;
      margin: 1.1rem 0 0 0;
      font-weight: 600;
    }
  }
}

@media only screen and (max-width: 1024px) {


  $small-text: 1.25rem;
  $p-text: 1.2rem;
  $big-text: 1.2rem;
  $h6-text: 1.7rem;
  $h5-text: 1.8rem;
  $h4-text: 2.9rem;
  $h3-text: 3.8rem;
  $h2-text: 4.9rem;
  $h1-text: 4rem;

.team-members{
  margin-top: 6rem;

  .team-member {
    max-width: 100%;
    width: 80%;
    margin-bottom: 4rem;
    padding: 1.7rem 0 1rem 0;

    // Effet Glass Morphism
    background: rgba($color: $terciary-white, $alpha: .5); // rgba(35,35,35,0.5)
    -webkit-backdrop-filter: blur(10px);
    backdrop-filter: blur(10px);
    border: 1px solid rgba($color: $terciary-white, $alpha: .25); //rgba(35,35,35,0.25)

    // Effet d'ombre
    box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

    // Ligne sur le côté
    &:after, &:before {
      position: absolute;
      content: '';
      top: .5rem;
      width: 2.5px;
      background-color: $primary-red;
      transform: skewY(30deg);
    }

    &:after {
      left: -.8rem;
      height: 5.3rem;
    }

    &:before {
      left: -1.4rem;
      height: 3rem;
    }

    &:not(.team-member:last-of-type) {
      margin-right: 0;
    }

    &:hover {
      transform: translateY(-8%);
    }

    .team-member-img {
      position: relative;
      margin: 0 auto;
      width: 6rem;
      height: 6rem;
      border-radius: $round;
      overflow: hidden;
      margin-bottom: 1.2rem;
      border: 6px solid $secondary-red;

      img {
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        height: 100%;
      }
    }

    h3, h4 {
      text-align: center;
    }

    h3 {
      position: relative;
      font-size: $h5-text;
      //color: $primary-red;
      margin: 0;

      &:after {
        position: absolute;
        content: '';
        bottom: -.55rem;
        left: 50%;
        transform: translateX(-50%);
        background-color: $secondary-red;
        width: 4.7rem;
        height: 2px;
        font-weight: 700;
      }
    }

    h4 {
      font-size: $p-text;
      color: $secondary-red;
      margin: 1.1rem 0 0 0;
      font-weight: 600;
    }
  }

}


}


</style>