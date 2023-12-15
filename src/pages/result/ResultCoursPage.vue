<template>
  <headerComp :selected="2"></headerComp>
  <main>
    <CoursResultSect></CoursResultSect>
  </main>
</template>

<script>
import headerComp from "@/components/header/HeaderComp.vue";
import CoursResultSect from "@/components/cours/result/section/CoursResultSect.vue";
export default ({
  components: {
    headerComp,
    CoursResultSect
  }
})
</script>

<style lang="scss">
@import '../../utils/computer/components.scss';

main {
  position: relative;
}

</style>