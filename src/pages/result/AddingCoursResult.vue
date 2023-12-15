<template>
  <headerComp :selected="2"></headerComp>
  <main>
    <AddSect></AddSect>
  </main>
</template>

<script>
import headerComp from "@/components/header/HeaderComp.vue";
import AddSect from "@/components/adding/section/AddSect.vue";
export default ({
  components: {
    headerComp,
    AddSect
  }
})
</script>

<style lang="scss">
@import '../../utils/computer/components.scss';

main {
}

</style>