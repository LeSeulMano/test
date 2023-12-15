<template>
  <headerComp :selected="5"></headerComp>
  <main>
    <AdminSect></AdminSect>
  </main>
</template>

<script>
import headerComp from "@/components/header/HeaderComp.vue";
import AdminSect from "@/components/admin/section/AdminSect.vue";
export default ({
  components: {
    headerComp,
    AdminSect
  }
})
</script>

<style lang="scss">
@import '../../utils/computer/components.scss';

main {
  overflow: hidden;
}

</style>