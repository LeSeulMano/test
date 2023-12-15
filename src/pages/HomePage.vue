<template>
  <headerComp :selected="1"></headerComp>

  <main>
    <LandingPage></LandingPage>
    <AboutSect></AboutSect>
    <StaffSelect></StaffSelect>
  </main>
  <FooterComponent></FooterComponent>
</template>

<script>
import LandingPage from "@/components/home/sections/LandingSect.vue";
import AboutSect from "@/components/home/sections/AboutSect.vue";
import StaffSelect from "@/components/home/sections/StaffSect.vue";
import FooterComponent from "@/components/footer/FooterComponent.vue";
import headerComp from "@/components/header/HeaderComp.vue";

export default {
  name: 'App',
  components: {
    headerComp,
    LandingPage,
    AboutSect,
    StaffSelect,
    FooterComponent

  }
}

</script>

<style lang='scss'>
main {
  position: relative;
}

@media only screen and (max-width: 1024px) {

  main {}
}
</style>