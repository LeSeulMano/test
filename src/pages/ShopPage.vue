<template>
  <headerComp :selected="4"></headerComp>
  <main class="main-soon">
    <SoonSect></SoonSect>
  </main>
  <FooterComponent></FooterComponent>
</template>

<script>

import {defineComponent} from "vue";
import headerComp from "@/components/header/HeaderComp.vue";
import SoonSect from "@/components/soon/SoonSect.vue";
import FooterComponent from "@/components/footer/FooterComponent.vue";

export default defineComponent({
  components: {FooterComponent, headerComp, SoonSect}
})
</script>

<style lang="scss">

</style>