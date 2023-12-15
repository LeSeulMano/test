<template>
  <div class="app">
    <nav>
      <router-link style="text-decoration: none; color: inherit;" to="/">
      </router-link>
    </nav>
    <router-view/>
    <CookieBanner @accept-cookies="activateGoogleAnalytics"></CookieBanner>
  </div>
</template>

<script>
import CookieBanner from '@/components/modal/CookieModal.vue';
export default {
  name: 'App',
  components: {
    CookieBanner
  },
  methods: {
    activateGoogleAnalytics() {
      if (this.$cookies.get('cookiesAccepted') === 'true') {
        // Inclure ici le code d'inclusion du script Google Analytics
        const script = document.createElement('script');
        script.src = 'https://www.googletagmanager.com/gtag/js?id=YOUR_ANALYTICS_ID';
        script.async = true;
        document.head.appendChild(script);

        script.onload = () => {
          // Initialiser Google Analytics
          window.dataLayer = window.dataLayer || [];
          function gtag() {
            window.dataLayer.push(arguments);
          }
          gtag('js', new Date());
          gtag('config', 'YOUR_ANALYTICS_ID');
        };
      }
    },
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,400;0,500;0,600;0,700;0,800;1,400;1,500;1,600;1,700;1,800&display=swap');
@import 'utils/computer/variables';
@import 'utils/computer/components';

#app {
  font-family: "Poppins", sans-serif;
  height: 100%;
  width: 100%;
  color: $primary-black;
  font-size: $p-text;
  overflow-x: hidden;
}

.app {
  max-width: 1920px;
  height: 100%;
  margin: 0px auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;

  nav{
    height: auto;
  }
}

body {
  overflow-x: hidden;
  margin: 0;
  background-color: $terciary-white;
}
</style>
