<template>
  <headerComp :selected="0"></headerComp>
  <main class="main-login">
    <LogSect></LogSect>
  </main>
</template>

<script>
import headerComp from "@/components/header/HeaderComp.vue";
import LogSect from "@/components/login/section/LogSect.vue";
export default {
  name: 'App',
  components: {
    headerComp,
    LogSect
  }
}

</script>

<style lang='scss'>

.main-login {
  background-image: url("../assets/login1.svg");
  background-size: cover;
  overflow: hidden;
}
</style>