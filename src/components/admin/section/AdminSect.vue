<template>
    <AdminHeader :selected="selectedPage" @selectPage="handleSelectPage"></AdminHeader>
    <AdminUser :adminUser="selectedPage === 0"></AdminUser>
    <AdminCours :adminUser="selectedPage === 1"></AdminCours>
</template>

<script>
import AdminHeader from "@/components/admin/header/AdminHeader.vue";
import AdminUser from "@/components/admin/users/AdminUser.vue";
import AdminCours from "@/components/admin/cours/AdminCours.vue";
export default {
  components: {
    AdminHeader,
    AdminUser,
    AdminCours
  },
  data() {
    return {
      selectedPage: 0,
    };
  },
  methods: {
    handleSelectPage(index) {
      this.selectedPage = index;
    },
  },
}
</script>

<style lang="scss">


</style>