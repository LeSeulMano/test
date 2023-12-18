<template>
  <section id="sect11">
    <AccountInfo :isShow="isAccountInfoVisible" @change-tab="showTab"></AccountInfo>
    <ChangePassword :isShow="isChangePasswordVisible" @change-tab="showTab"></ChangePassword>
    <ChangeUsername :isShow="isChangeUsernameVisible" @change-tab="showTab"></ChangeUsername>
    <ChangeEmail :isShow="isChangeEmailVisible" @change-tab="showTab"></ChangeEmail>
  </section>
</template>

<script>
import AccountInfo from "@/components/account/section/AccountInfo.vue";
import ChangePassword from "@/components/account/section/ChangePassword.vue";
import ChangeUsername from "@/components/account/section/ChangeUsername.vue";
import ChangeEmail from "@/components/account/section/ChangeEmail.vue";
export default {
  components: {
    AccountInfo,
    ChangePassword,
    ChangeUsername,
    ChangeEmail
  },
  data() {
    return {
      isAccountInfoVisible: true,
      isChangePasswordVisible: false,
      isChangeUsernameVisible: false,
      isChangeEmailVisible: false
    }
  },
  methods: {
    showTab(tabName) {
      switch (tabName) {
        case 'ChangePassword':
          this.isAccountInfoVisible = false;
          this.isChangePasswordVisible = true;
          this.isChangeUsernameVisible = false;
          this.isChangeEmailVisible = false;
          break;
        case 'InfoUser':
          this.isAccountInfoVisible = true;
          this.isChangePasswordVisible = false;
          this.isChangeUsernameVisible = false;
          this.isChangeEmailVisible = false;
          break;
        case 'ChangeUsername':
          this.isAccountInfoVisible = false;
          this.isChangePasswordVisible = false;
          this.isChangeUsernameVisible = true;
          this.isChangeEmailVisible = false;
          break;
        case 'ChangeEmail':
          this.isAccountInfoVisible = false;
          this.isChangePasswordVisible = false;
          this.isChangeUsernameVisible = false;
          this.isChangeEmailVisible = true;
          break;
      }
    }
  }
}

</script>

<style lang="scss">
@import "../../../utils/computer/components";

#sect11 {
  position: relative;
  overflow: hidden;
  padding: 0px;
  display: flex;
  justify-content: center;
  align-items: center;

  &::before {
    position: absolute;
    content: '';
    height: 100%;
    width: 101%;
    bottom: -.5rem;
    background-image: url("../../../assets/wave1.svg");
    background-repeat: no-repeat;
    background-position-y: 100%;
    left: 50%;
    transform: translateX(-50%);
  }

}
</style>