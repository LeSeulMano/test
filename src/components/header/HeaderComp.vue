<template>
  <header id="header">
    <router-view/>
    <nav id="header-nav" class="header-nav">
      <ul id="ul-header">
        <li>
          <router-link class="router-link" :class="selected == 1 ? 'onPage' : ''" to="/">Accueil</router-link>
        </li>
        <li>
          <router-link class="router-link" :class="selected == 2 ? 'onPage' : ''" to="/cours">Cours</router-link>
        </li>
        <li class="nav-logo" id="nav-logo">
          <router-link :class="{ 'onSelected': isMenuOpen }" class="router-link logo-link" to="/" id="navLogo">
            <img src="../../assets/Logo_DELMOO.png" alt="Logo de l'association Delmoo" id="navLogoImg">
          </router-link>
        </li>
        <li>
          <router-link class="router-link" :class="selected == 3 ? 'onPage' : ''" to="/podcast">Podcast</router-link>
        </li>
        <li>
          <router-link class="router-link " :class="selected == 4 ? 'onPage' : ''" to="/shop">Shop</router-link>
        </li>
      </ul>
      <div class="burger-menu" id="menu-opener" @click="menuOpener">
        <div></div>
        <div></div>
        <div></div>
      </div>
    </nav>
    <div class="account">
      <router-link to="/login" class="account-icon router-link not-connected login" data-tooltip="Connexion"><IonIcon name="person"></IonIcon></router-link>
      <div class="account-icon connected logout" @click="logout" data-tooltip="Déconnexion"><IonIcon name="log-out"></IonIcon></div>
      <router-link to="/account" class="router-link connected account-icon compte" data-tooltip="Compte"><IonIcon name="document"></IonIcon></router-link>
    </div>
  </header>

</template>

<script>
import { IonIcon } from "@ionic/vue";
import axios from "axios";

export default {
  components: {
    IonIcon
  },
  data() {
    const screenHeight = window.innerHeight;
    var headerShrinked = false;
    return {
      screenHeight,
      headerShrinked,
      isMenuOpen: false,
    };
  },
  props:{
    selected: Number,
  },
  mounted() {
    window.addEventListener('scroll', this.handleScroll);
    axios.get("https://delmoo.fr:5000/connect", {
      withCredentials: true,
      validateStatus: function (status) {
        return status === 401 || status === 200;
      }
    }).then((res) => {
      if (res.status == 200){
        document.querySelectorAll(".connected")[0].classList.add('selected');
        document.querySelectorAll(".connected")[1].classList.add('selected');
        document.querySelectorAll(".not-connected")[0].classList.remove('selected');
      } else {
        document.querySelectorAll(".connected")[0].classList.remove('selected');
        document.querySelectorAll(".connected")[1].classList.remove('selected');
        document.querySelectorAll(".not-connected")[0].classList.add('selected');
      }
    }).catch(() => {
      document.querySelectorAll(".connected")[0].classList.remove('selected');
      document.querySelectorAll(".connected")[1].classList.remove('selected');
      document.querySelectorAll(".not-connected")[0].classList.add('selected');
    })
  },
  methods: {
    logout(){
      axios.get('https://delmoo.fr:5000/logout', {
        withCredentials: true
      }).then(() => {
        window.location.reload();
      })
    },
    handleScroll() {
      const header = document.querySelector('header');
      const logo = document.getElementById('navLogo');

      if (document.documentElement.scrollTop > 5 && !document.getElementById('menu-opener').classList.contains('opened')) {
        header.classList.add("header-shrink");
        logo.classList.add('header-logo-shrink');
      } else if (!document.getElementById('menu-opener').classList.contains('opened')) {
        header.classList.remove("header-shrink");
        logo.classList.remove('header-logo-shrink');
      }
      if (window.matchMedia("(max-width: 1024px)").matches) {

        document.addEventListener('click', function (e) {
          if (!document.querySelector('header').contains(e.target) && document.getElementById('menu-opener').classList.contains('opened')) {
            this.closeMenu();
          }
        });
      }
    },

    menuOpener(event) {
      if (event.target.classList.contains('opened')) {
        this.closeMenu();
      } else {
        this.openMenu();
      }
    },

    openMenu() {
      const header = document.getElementById('header');
      const logo = document.getElementById("nav-logo");

      header.classList.remove("header-shrink");
      logo.classList.remove('header-logo-shrink')

      header.style.backgroundColor = 'rgb(249, 243, 241)'; // Fond beige
      header.style.height = this.screenHeight + 'px';
      document.getElementById('header-nav').style.height = this.screenHeight + 'px';
      document.getElementById('ul-header').style.height = this.screenHeight + 'px';

      const navListItems = document.querySelectorAll('nav ul li');
      navListItems.forEach(item => {
        item.style.opacity = '1';
        item.style.visibility = 'visible';
      });

      document.getElementById('menu-opener').classList.add('opened');

      document.querySelector('.nav-logo').style.top = '7rem'
      document.querySelector('.nav-logo img').style.width = '8rem'
      document.querySelector('.nav-logo img').style.height = '8rem'
      document.querySelector('#menu-opener > div:nth-of-type(1)').style.transform = 'translateY(7.75px) rotate(-45deg)';
      document.querySelector('#menu-opener > div:nth-of-type(2)').style.opacity = '0';
      document.querySelector('#menu-opener > div:nth-of-type(3)').style.transform = 'translateY(-7.75px) rotate(45deg)';
    },

    closeMenu() {

      const header = document.getElementById('header');
      const logo = document.getElementById("nav-logo");

      if (this.headerShrinked) {
        header.classList.add("header-shrink");
        logo.classList.add('header-logo-shrink')
      }
      header.style.height = '4.6rem';

      const navListItems = document.querySelectorAll('nav ul li');
      navListItems.forEach(item => {
        if (!item.classList.contains('nav-logo')) {
          item.style.opacity = '0';
          item.style.visibility = 'hidden';
        }
      });

      document.getElementById('menu-opener').classList.remove('opened');

      document.querySelector('.nav-logo').style.top = '5rem'
      document.querySelector('.nav-logo img').style.width = '5.2rem'
      document.querySelector('.nav-logo img').style.height = '5.2rem'
      document.querySelector('#menu-opener > div:nth-of-type(1)').style.transform = 'translateY(0) rotate(0deg)';
      document.querySelector('#menu-opener > div:nth-of-type(2)').style.opacity = '1';
      document.querySelector('#menu-opener > div:nth-of-type(3)').style.transform = 'translateY(0) rotate(0deg)';

      setTimeout(() => {
        document.getElementById('header-nav').style.height = '0px';
        document.getElementById('ul-header').style.height = '0px';
        document.getElementById('ul-header').style.visibility = 'hidden'
      }, 400);
    }
  },
};
</script>

<style lang="scss">
@import '../../utils/computer/components';

header {
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
  height: 7rem;
  width: 100%;
  background-color: $terciary-white;
  transition: .3s;

  .header-nav {

    display: flex;

    .nav-logo {
      position: relative;
      width: 10rem;
      transition: .3s;

      img {
        position: absolute;
        transition: .3s;
        top: -3.2rem;
        left: 50%;
        transform: translateX(-50%);
        width: 7rem;
        height: 7rem;
        filter: drop-shadow(0 0 0.2rem $terciary-black);
      }
    }

    ul {
      display: flex;
      align-items: center;
      padding: 0;
      margin: 0;

      li {
        list-style: none;
        margin-right: 2.5rem;
        font-weight: 600;

        a:not(.logo-link) {
          position: relative;
          display: block;
          padding: .6rem .9rem;
          z-index: 2;
          color: $primary-red;
          font-size: $p-text;

          &.onPage {
            color: $secondary-red;
          }

          &.onPage::after {
            position: absolute;
            content: '';
            height: 2px;
            width: 50%;
            top: 80%;
            left: 50%;
            transform: translateX(-50%);
            background-color: $secondary-red;
          }
        }

        a:not(.onPage, .logo-link)::after {
          position: absolute;
          content: '';
          height: 2px;
          width: 0%;
          top: 80%;
          left: 50%;
          transform: translateX(-50%);
          background-color: $primary-red;
          transition: .3s;
        }

        a:not(.onPage):hover::after {
          width: 50%;
        }

      }
    }


    .burger-menu {
      display: none;
    }
  }

  .account {
    position: absolute;
    right: 0;
    text-decoration: none;
    color: inherit;
    margin-right: 1rem;
    padding: 1rem;
    display: flex;

    .account-icon {
      margin-right: 1.4rem;
      font-size: 1.1rem;
      cursor: pointer;
      color: black;

      &:not(.selected) {
        display: none;
      }
    }
    .account-icon::after{
      content: attr(data-tooltip);
      position: absolute;
      bottom: -25px;
      transform: translateX(-50%);
      background-color: rgba(249, 243, 241, 0.5);
      box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
      backdrop-filter: blur(10px);
      color: rgb(75, 75, 75);
      font-weight: bold;
      padding: 5px;
      font-size: 1rem;
      border-radius: 5px;
      opacity: 0;
      transition: opacity 0.3s;
      white-space: nowrap;

      &.compte{
        left: 60%;
      }
      &.logout{
        left: 30%
      }
      &.login{
        left: 40%;
      }

    }
    .account-icon:hover::after{
      opacity: 1;
    }
    .logout{
      font-size: 1.2rem;
    }
  }
}

@media only screen and (max-width: 1024px) {

  header {
    display: block;
    height: 4.6rem;
    width: 100%;
    background-color: $terciary-white;
    transition: .4s;

    .header-nav {
      position: relative;
      display: block;
      width: 85%;
      margin: 0 auto;
      padding: .4rem 0;
      height: 1px;

      .nav-logo {
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        top: 5rem;
        width: 10rem;
        transition: .4s;

        img {
          position: absolute;
          transition: .4s;
          top: -3.5rem;
          left: 50%;
          transform: translateX(-50%);
          width: 5.2rem;
          height: 5.2rem;
          filter: drop-shadow(0 0 0.2rem $terciary-black);
        }
      }

      ul {
        position: absolute;
        display: flex;
        flex-direction: column;
        justify-content: center;
        left: 50%;
        transform: translate(-50%);
        top: -1rem;
        text-align: center;
        z-index: 1;
        padding: 0;
        margin: 0;
        align-items: center;
        text-align: center;

        li {
          margin-right: 0;
          transition: .3s;
          list-style: none;
          font-weight: 600;

          a:not(.logo-link) {
            position: relative;
            display: block;
            padding: .6rem .7rem;
            z-index: 2;
            color: $primary-red;
            font-size: $p-text;

            &.onPage {
              color: $secondary-red;
            }

            &.onPage::after {
              position: absolute;
              content: '';
              height: 2px;
              width: 50%;
              top: 80%;
              left: 50%;
              transform: translateX(-50%);
              background-color: $secondary-red;
            }
          }

          a:not(.onPage)::after {
            position: absolute;
            content: '';
            height: 2px;
            width: 0%;
            top: 80%;
            left: 50%;
            transform: translateX(-50%);
            transition: .3s;
          }

          a:not(.onPage):hover::after {
            width: 50%;
          }

          .onPage::after {
            position: absolute;
            content: '';
            height: 2px;
            width: 50%;
            top: 80%;
            left: 50%;
            transform: translateX(-50%);
            background-color: $secondary-red;
          }
        }

        li:not(.nav-logo) {
          visibility: hidden;
          opacity: 0;
        }
      }

      .burger-menu {
        position: relative;
        left: 0;
        display: flex;
        z-index: 2;
        justify-content: space-between;
        flex-direction: column;
        height: 1.3rem;
        width: 1.8rem;
        top: .4rem;
        padding: 1rem;

        div {
          position: relative;
          width: 100%;
          height: 2px;
          background-color: $primary-red;
          transform: skewX(30deg);
          transition: .4s;
        }
      }

    }
    .account{
      position: absolute;
      top: .6rem;
      padding: 1rem;
      right: 0;

      .account-icon {
        margin-right: 1.4rem;
        cursor: pointer;
        color: black;

        ion-icon{
          font-size: 1.2rem;
        }

        &:not(.selected) {
          display: none;
        }
      }
    }

  }

}
</style>