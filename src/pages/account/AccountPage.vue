<template>
  <headerComp :selected="0"></headerComp>
  <main>
    <AccountSect></AccountSect>
  </main>
</template>

<script>

import headerComp from "@/components/header/HeaderComp.vue";
import AccountSect from "@/components/account/page/AccountSect.vue";
export default ({
  components: {headerComp, AccountSect}
})
</script>

<style lang="scss">
@import '../../utils/computer/components';

main{
  overflow: hidden;
}

@media only screen and (max-width: 1024px) {
  main {}
}

</style>